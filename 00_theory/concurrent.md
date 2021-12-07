### 编译时指令重排
参见[文章](https://preshing.com/20120625/memory-ordering-at-compile-time/)

#### 抓住那个重排犯
参见[文章](https://preshing.com/20120515/memory-reordering-caught-in-the-act/)
- `asm volatile("" ::: "memory");`仅仅`只限制编译器重排`，对cpu重排不做限制
- `asm volatile("mfence" ::: "memory");`限制内存重排，是一个` a full memory barrier`
- `pthread_setaffinity_np`还可以通过限制多线程只在一个cpu上执行实现

特别的，`mfence`指令是`x86/64`独有的。linux内核使用`smp_mb`包裹之, 且有变种`smp_rmb`and`smp_wmb`

### 内存屏障
参见[文章](https://preshing.com/20120710/memory-barriers-are-like-source-control-operations/)

场景模拟：多核架构如下

![cpu-diagram](./res/cpu-diagram.png)

将多cpu线程间的协作类比为多人协作编程

![source-control-analogy](./res/source-control-analogy.png)

两者工作若互相独立时内存可以无需屏障，其重要点凸显在需要访问共享变量时，需要通过内存屏障保证协同性（例如一般会使用多个变量，一个作为flag）

#### 内存屏障类型
抽象上可分为四种类型，不同cpu指令是这些类型组合。

![barrier-types](./res/barrier-types.png)

1. `#LoadLoad`保证屏障前后读操作不重排。e.g.使得flag成功判定后再去读真实数据。(注意这俩都是共享变量，需要原子操作)
2. `#StoreStore`保证屏障前后写操作不重排。e.g.与上类似，保证正确更新数据后再去更新flag通知变量，保证数据的一致性
3. `#LoadStore`保证屏障前读操作不重排到屏障后写操作之后。e.g.cache missing时可能发生先写后读的指令重排，使用该屏障可以限制相关操作。注意理论上前两者组合并不能得到该屏障，虽然实际实现上该屏障大多内含前两者之一。
4.`#StoreLoad`保证屏障前的写操作不重排到读操作之后。e.g.特别是在`r1=r2=0`的场景下，只有该屏障能保证正确结果。但注意似乎该屏障开销较大，略似完全一致性顺序。

另有[论文](http://www.rdrop.com/users/paulmck/scalability/paper/whymb.2010.07.23a.pdf)一篇


### 移动获取语义
参见[文章](https://preshing.com/20120913/acquire-and-release-semantics/)

定义可参考以下一种说法

    Acquire semantics is a property that can only apply to operations that read from shared memory, whether they are read-modify-write operations or plain loads. The operation is then considered a read-acquire. Acquire semantics prevent memory reordering of the read-acquire with any read or write operation that follows it in program order.

    Release semantics is a property that can only apply to operations that write to shared memory, whether they are read-modify-write operations or plain stores. The operation is then considered a write-release. Release semantics prevent memory reordering of the write-release with any read or write operation that precedes it in program order.

总结下即

- 获取语义限制`读操作之后`的指令被重排到前面，保证fence之前的值已被获取，例如获取锁
- 释放语义限制`写操作之前`的指令重排到后面，保证fence之后所有指令被执行完毕，例如释放锁

特别可参见该图

![acq-rel-barriers](./res/acq-rel-barriers.png)

例子如下，通过内存屏障`保证两线程获取数据的一致性`（例子中，当thread2读到r1==1时，r2一定也已经被赋值为42。Thread1生产者`两个连续写`间使用release保证其顺序一致，Thread2`两个连续读`间使用acquire保证不重排，实际Ready变量这里应该是个flag通知变量，Thread2应先对其做判断）

![cpp11-fences](./res/cpp11-fences.png)

acq-rel锁保证了临界区的实现

![acq-rel-lock](./res/acq-rel-lock.png)

### 无锁编程
参见[文章](https://preshing.com/20120612/an-introduction-to-lock-free-programming/)

简单定义lock-free programming:

![its-lock-free](./res/its-lock-free.png)

涉及技术及选择方式如下

![techniques](./res/techniques.png  )

#### 原子性操作

1. 简单变量的aligned read and write一般是原子的
2. `RMW`则更进一步，适合于有多个写者时（此时会形成一个队列并顺序访问相同地址），C++中则有`std::atomic<int>::fetch_add`。注意C++中并不保证某指令在当前平台一定是无锁的，可以通过`std::atomic<>::is_lock_free`进行判断。注意即使是单线程，有多个写者时也需要使用原子性操作保证事务的完整性。
3. `CAS`一般用在循环中来反复进行，先将shared variable赋值给local variable，并对其做自己想要的更改，再验证与旧值相同时进行更新，不同时至少说明其他线程刚成功更新该值。
4. 顺序一致性。最简单的方式是关闭编译器优化并强制线程在单核上运行（因为单处理器自身不会让自己的结果不一致，即使发生了抢占调度）。C++11中原子变量类型能保证顺序一致性，一般靠内存屏障或RMW操作实现。
5. 内存排序，作者分为三种类别，即轻量级fence，全内存屏障和移动获取语义。(注：Acquire semantics prevent memory reordering of operations which follow it in program order, and release semantics prevent memory reordering of operations preceding it. 该方式非常适合生产者消费者模型，其中一个生产者发布信息，其他线程进行读取)

`CAS`实现的无锁队列如下
```
void LockFreeQueue::push(Node* newHead)
{
    for (;;)
    {
        // Copy a shared variable (m_Head) to a local.
        Node* oldHead = m_Head;

        // Do some speculative work, not yet visible to other threads.
        newHead->next = oldHead;

        // Next, attempt to publish our changes to the shared variable.
        // If the shared variable hasn't changed, the CAS succeeds and we return.
        // Otherwise, repeat.
        if (_InterlockedCompareExchange(&m_Head, newHead, oldHead) == oldHead)
            return;
    }
}
```

`CAS`实现一个简单的`不阻塞式锁`（感觉相当于`try_lock()`）如下。可以看到通过原子变量的`CAS+aquire语义`实现`加锁`,通过`release语义`实现解锁。(该例子来自[该文](https://preshing.com/20121019/this-is-why-they-call-it-a-weakly-ordered-cpu/))
```
void IncrementSharedValue10000000Times(RandomDelay& randomDelay)
{
    int count = 0;
    while (count < 10000000)
    {
        randomDelay.doBusyWork();
        int expected = 0;//这两句是在获取锁
        if (flag.compare_exchange_strong(expected, 1, memory_order_acquire))
        {
            // Lock was successful
            sharedValue++;
            flag.store(0, memory_order_release);//这里进行释放
            count++;
        }
    }
}
```

一般说来，x86/64架构是强顺序一致性（`所有读操作都含有acquire语义，所有写操作都含有release语义`），而ARM/PowerPC等属于弱一致性会进行一些重排

### 有锁多线程
关于如何运用mutex互斥锁或其他高级同步对象如semaphores信号量和事件等

造成死锁的可能性有，ABBA型等

有些特殊情况要注意下，如
- 线程获取锁后被调度/中断

### RCU
详见[文档](https://www.kernel.org/doc/Documentation/RCU/whatisRCU.txt)

#### what is RCU
RCU is a synchronization mechanism that was added to the Linux kernel
during the 2.5 development effort that is optimized for read-mostly
situations. 

涉及如下问题

1.	RCU OVERVIEW
2.	WHAT IS RCU'S CORE API?
3.	WHAT ARE SOME EXAMPLE USES OF CORE RCU API? (example uses)
4.	WHAT IF MY UPDATING THREAD CANNOT BLOCK? (example uses)
5.	WHAT ARE SOME SIMPLE IMPLEMENTATIONS OF RCU?
6.	ANALOGY WITH READER-WRITER LOCKING
7.	FULL LIST OF RCU APIs
8.	ANSWERS TO QUICK QUIZZES

#### 1.  RCU OVERVIEW

RCU的基本思想是将`update更新`操作分解为`removal移除`操作和`reclamation回收`操作。移除阶段将移除数据项的引用，且可以与读者同步进行，这依赖于现代CPU确保读者读取到完整非撕裂的数据。回收阶段将在所有读者不再引用数据项后进行。

上述分解使得写者能够立即进行移除操作，并在回收阶段`通过阻塞等待或注册回调函数`延后到`所有移除阶段活跃的读者结束读取`。注意，任何在移除阶段后的读者将不会得到目标项的引用，也就无法打断回收阶段。

所以RCU一般性更新序列即为：

a. 移除指针(removal phase)
b. 等待前序读者结束读端临界区
c. 释放指针(reclamation phase)

b项是延迟销毁的关键点，这种等待使得RCU读者可以使用用非常轻量的同步方式，甚至无需同步。作为对比，在传统基于锁的实现中，读者必须使用重量级同步才能防止写者篡改数据，因为传统方式一般原地更新数据项，所以要做互斥处理。而RCU写者利用了现代CPU原子性更新指针的优势，使得能够在不打扰读者的情况下原子性插入、移除或替换链表节点。RCU读者则能够继续访问旧数据，并能够免除原子操作、内存屏障、cache misses甚至是锁竞争这些在现代SMP计算机系统中代价颇大的行为。

#### 2.  WHAT IS RCU'S CORE API?

The core RCU API is quite small:

a.	`rcu_read_lock()`: 读者通知reclaimer释放线程，读者已进入读端临界区。(在RCU读端关键部分阻塞是非法的，尽管使用CONFIG_PREEMPT_RCU构建的内核可以抢占RCU读取端临界区。在 RCU 读端临界区期间访问的任何受 RCU 保护的数据结构都保证在该临界区的整个持续时间内保持不被回收。 引用计数可以与RCU结合使用，以维护对数据结构的长期引用。)
b.	`rcu_read_unlock()`:读者通知reclaimer释放线程,读者已离开读端临界区。请注意，RCU 读取端临界区可能会嵌套和/或重叠。
c.	`synchronize_rcu()` / `call_rcu()`: `synchronize_rcu()`标记更新代码的结束和回收代码的开始。 它会`阻塞`到所有CPU上的所有之前进入的RCU读取端临界区都退出。 注意，它不一定要等待任何后续的RCU读端临界区完成，只等待正在进行的读端临界区完成。同时，它也不是立即执行的，而是有调度时延。并且很多实现上都会使用批处理请求的方式来提高效率，这也引入了时延。  因为该函数会侦测读端何时结束，其实现非常重要。`call_rcu()`则是前者的回调函数形式。该形式在不能阻塞或更新端性能非常重要时非常有用。 但是，不应轻易使用`call_rcu()` API，因为使用`synchronized_rcu()` API通常会产生更简单的代码。 此外，如果宽限期延迟, `synchronize_rcu()` API具有自动限制更新率的良好属性。 此属性导致系统在面对拒绝服务攻击时具有弹性。 使用`call_rcu()`的代码应该限制更新率以获得同样的弹性。 有关限制更新速率的一些方法，请参阅 checklist.txt。
d.	`rcu_assign_pointer()`:该函数被实现为宏。updater使用该函数来给RCU保护的指针赋一个新值，以使得能够将值得变通从updater通知到reader。该宏不会计算为右值，但它会执行特定CPU架构所需的任何内存屏障指令。 也许同样重要的是，它用于记录 (1) 哪些指针受 RCU 保护，以及 (2) 给定结构可被其他 CPU 访问的点。 也就是说， rcu_assign_pointer() 最常被 _rcu 链表操作原语，例如 list_add_rcu()间接使用，。

e.	`rcu_dereference()`：和前者一样，也必须被实现为宏。读者使用该API来获取一个被RCU保护得指针，该指针返回一个可以安全解引用的值。 请注意， rcu_dereference() 实际上并没有对指针解引用，相反，它保护指针以供以后解引用。 它还为给定的 CPU 架构执行任何所需的内存屏障指令。 目前，只有 Alpha 需要 rcu_dereference() 中的内存屏障——在其他 CPU 上，它编译为空，甚至不需要编译器指令。 一般情况下时将返回值赋值给一个局部指针变量，并对局部指针解引用。请注意，rcu_dereference() 返回的值仅在封闭的 RCU 读端临界区内有效。与 rcu_assign_pointer() 一样，rcu_dereference() 的一个重要功能是记录哪些指针受 RCU 保护，特别是标记随时可能更改的指针，包括在 rcu_dereference() 之后立即更改。 而且，与 rcu_assign_pointer() 一样，rcu_dereference() 通常通过 _rcu 列表操作原语间接使用，例如 list_for_each_entry_rcu()。

3.  WHAT ARE SOME EXAMPLE USES OF CORE RCU API?

略

主要以问题点在于

1. synchronize_rcu发生在reader之前，reader会阻塞吗，如何保证读到的数据一致。即写者仍会受读者阻塞但读者不会受到写者阻塞吗。

        rcu_assign_pointer(gp, NULL);
        spin_unlock(&gp_lock);
        synchronize_rcu();

答： 应该不会，旧的reader直接督导old_gp，新来的reader读到新的new_gp，完全不搭界，因为对gp的访问仍都是原子的。synchronize_rcu只是在等待宽限期后释放原节点而已。

2. 发布订阅机制的有效性依靠dereference()和assign_pointer()保证。

3. 
| Given that multiple CPUs can start RCU read-side critical sections at |
| any time without any ordering whatsoever, how can RCU possibly tell   |
| whether or not a given RCU read-side critical section starts before a |
| given instance of ``synchronize_rcu()``?                              |

参考[文档](https://www.kernel.org/doc/Documentation/RCU/Design/Requirements/Requirements.html)

### C/C++ 
[C memory order](https://en.cppreference.com/w/c/atomic/memory_order)   



### 疑问
1. x86/64是strong memory ordered的话，那其实不是一般都不用考虑重排？
2. 为啥不重排性能还高一点？
3. data-dependency reordering
4. 关于[consume语义](https://preshing.com/20140709/the-purpose-of-memory_order_consume-in-cpp11/)
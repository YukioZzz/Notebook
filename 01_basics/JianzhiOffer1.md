## 第二章 基础知识
### 2.2 编程语言
1. `sizeof()`一个空类型大致返回结果时1，尽管其不包含任何信息，具体由编译器决定; 如果类型中有虚函数，则会引入虚函数指针，其占用字节根据机器位数有所不同
2. C++标准`不允许复制构造函数传值参数`，因为会造成递归调用。e.g. `b.A(a)=b.A(other.A(a))=b.A(other.A(other.A(a)))`
3. `面试题1：赋值运算符函数`：基础版本需要考虑 ①返回值类型声明为自身的引用，并返回*this ②传入参数声明为常量引用 ③判断是否原本就是同一个实例 ④是否释放自身原有内存； 进阶版本需考虑`异常安全性`，原版本在new时若无足够空间则返回空指针容易导致崩溃，则可采取两种方式改进 ①先new再delete，保证原实例不被修改 ②先创建临时实例，再进行交换。（此处借用了原类型的构造和析构函数）
4. `singleton模式`，演化路线`单线程简单版本`->`多线程加锁版本`->`加锁前后两次判断优化版本`->`细节：内存屏障问题`（此时要使用中间变量tmp）->`梅耶静态`(见[DesignPattern](./DesignPattern.md))

```C++
class Singleton{
private:
    std::atomic<Singleton*> m_instance;
    std::mutex m_mutex;
public:
    Singleton* Singleton::getInstance(){
        Singleton* tmp = m_instance.load(std::memory_order_relaxed);    //直接尝试读，使用tmp隔离对m_instance的变化
        std::atmoic_thread_fence(std::memory_order_acquire);            //实际为one-way barrier，可进不可出(roach motel)
        if (tmp == nullptr) {
            std::lock_guard<std::mutex> lock(m_mutex);                  //RAII
            tmp = m_instance.load(std::memory_order_relaxed);           //加锁后再读
            if (tmp == nullptr) {
                tmp = new Singleton;
                std::atomic_thread_fence(std::memory_order_release);
                m_instance.store(tmp, std::memory_order_relaxed);      //确认步骤
            }
        }
        return tmp;
    }
}
```

### 2.3 数据结构

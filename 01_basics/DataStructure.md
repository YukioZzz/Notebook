
<a id="data-structure"></a>

## 〽️ 数据结构

### 顺序结构

#### 顺序栈（Sequence Stack）

[SqStack.cpp](DataStructure/SqStack.cpp)

顺序栈数据结构和图片

```cpp
typedef struct {
	ElemType *elem;
	int top;
	int size;
	int increment;
} SqStack;
```

![](https://gitee.com/huihut/interview/raw/master/images/SqStack.png)

#### 队列（Sequence Queue）

队列数据结构

```cpp
typedef struct {
	ElemType * elem;
	int front;
	int rear;
	int maxSize;
}SqQueue;
```

##### 非循环队列

非循环队列图片

![](https://gitee.com/huihut/interview/raw/master/images/SqQueue.png)

`SqQueue.rear++`

##### 循环队列

循环队列图片

![](https://gitee.com/huihut/interview/raw/master/images/SqLoopStack.png)

`SqQueue.rear = (SqQueue.rear + 1) % SqQueue.maxSize`

#### 顺序表（Sequence List）

[SqList.cpp](DataStructure/SqList.cpp)

顺序表数据结构和图片

```cpp
typedef struct {
	ElemType *elem;
	int length;
	int size;
	int increment;
} SqList;
```

![](https://gitee.com/huihut/interview/raw/master/images/SqList.png)


### 链式结构

[LinkList.cpp](DataStructure/LinkList.cpp)

[LinkList_with_head.cpp](DataStructure/LinkList_with_head.cpp)

链式数据结构

```cpp
typedef struct LNode {
    ElemType data;
    struct LNode *next;
} LNode, *LinkList; 
```

#### 链队列（Link Queue）

链队列图片

![](https://gitee.com/huihut/interview/raw/master/images/LinkQueue.png)

#### 线性表的链式表示

##### 单链表（Link List）

单链表图片

![](https://gitee.com/huihut/interview/raw/master/images/LinkList.png)

##### 双向链表（Du-Link-List）

双向链表图片

![](https://gitee.com/huihut/interview/raw/master/images/DuLinkList.png)

##### 循环链表（Cir-Link-List）

循环链表图片

![](https://gitee.com/huihut/interview/raw/master/images/CirLinkList.png)

### 哈希表

[HashTable.cpp](DataStructure/HashTable.cpp)

#### 概念

哈希函数：`H(key): K -> D , key ∈ K`

#### 构造方法

* 直接定址法
* 除留余数法
* 数字分析法
* 折叠法
* 平方取中法

#### 冲突处理方法

* 链地址法：key 相同的用单链表链接
* 开放定址法
    * 线性探测法：key 相同 -> 放到 key 的下一个位置，`Hi = (H(key) + i) % m`
    * 二次探测法：key 相同 -> 放到 `Di = 1^2, -1^2, ..., ±（k)^2,(k<=m/2）`
    * 随机探测法：`H = (H(key) + 伪随机数) % m`

#### 线性探测的哈希表数据结构

线性探测的哈希表数据结构和图片

```cpp
typedef char KeyType;

typedef struct {
	KeyType key;
}RcdType;

typedef struct {
	RcdType *rcd;
	int size;
	int count;
	bool *tag;
}HashTable;
```

![](https://gitee.com/huihut/interview/raw/master/images/HashTable.png)

### 递归

#### 概念

函数直接或间接地调用自身

#### 递归与分治

* 分治法
    * 问题的分解
    * 问题规模的分解
* 折半查找（递归）
* 归并排序（递归）
* 快速排序（递归）

#### 递归与迭代

* 迭代：反复利用变量旧值推出新值
* 折半查找（迭代）
* 归并排序（迭代）

#### 广义表

##### 头尾链表存储表示

广义表的头尾链表存储表示和图片

```cpp
// 广义表的头尾链表存储表示
typedef enum {ATOM, LIST} ElemTag;
// ATOM==0：原子，LIST==1：子表
typedef struct GLNode {
    ElemTag tag;
    // 公共部分，用于区分原子结点和表结点
    union {
        // 原子结点和表结点的联合部分
        AtomType atom;
        // atom 是原子结点的值域，AtomType 由用户定义
        struct {
            struct GLNode *hp, *tp;
        } ptr;
        // ptr 是表结点的指针域，prt.hp 和 ptr.tp 分别指向表头和表尾
    } a;
} *GList, GLNode;
```

![](https://gitee.com/huihut/interview/raw/master/images/GeneralizedList1.png)

##### 扩展线性链表存储表示

扩展线性链表存储表示和图片

```cpp
// 广义表的扩展线性链表存储表示
typedef enum {ATOM, LIST} ElemTag;
// ATOM==0：原子，LIST==1：子表
typedef struct GLNode1 {
    ElemTag tag;
    // 公共部分，用于区分原子结点和表结点
    union {
        // 原子结点和表结点的联合部分
        AtomType atom; // 原子结点的值域
        struct GLNode1 *hp; // 表结点的表头指针
    } a;
    struct GLNode1 *tp;
    // 相当于线性链表的 next，指向下一个元素结点
} *GList1, GLNode1;
```

![](https://gitee.com/huihut/interview/raw/master/images/GeneralizedList2.png)

### 二叉树

[BinaryTree.cpp](DataStructure/BinaryTree.cpp)

#### 性质

1. 非空二叉树第 i 层最多 2<sup>(i-1)</sup> 个结点 （i >= 1）
2. 深度为 k 的二叉树最多 2<sup>k</sup> - 1 个结点 （k >= 1）
3. 度为 0 的结点数为 n<sub>0</sub>，度为 2 的结点数为 n<sub>2</sub>，则 n<sub>0</sub> = n<sub>2</sub> + 1
4. 有 n 个结点的完全二叉树深度 k = ⌊ log<sub>2</sub>(n) ⌋ + 1 
5. 对于含 n 个结点的完全二叉树中编号为 i （1 <= i <= n） 的结点
    1. 若 i = 1，为根，否则双亲为 ⌊ i / 2 ⌋
    2. 若 2i > n，则 i 结点没有左孩子，否则孩子编号为 2i
    3. 若 2i + 1 > n，则 i 结点没有右孩子，否则孩子编号为 2i + 1

#### 存储结构

二叉树数据结构

```cpp
typedef struct BiTNode
{
    TElemType data;
    struct BiTNode *lchild, *rchild;
}BiTNode, *BiTree;
```

##### 顺序存储

二叉树顺序存储图片

![](https://gitee.com/huihut/interview/raw/master/images/SqBinaryTree.png)

##### 链式存储

二叉树链式存储图片

![](https://gitee.com/huihut/interview/raw/master/images/LinkBinaryTree.png)

#### 遍历方式

* 先序遍历
* 中序遍历
* 后续遍历
* 层次遍历

#### 分类

* 满二叉树
* 完全二叉树（堆）
    * 大顶堆：根 >= 左 && 根 >= 右
    * 小顶堆：根 <= 左 && 根 <= 右
* 二叉查找树（二叉排序树）：左 < 根 < 右
* 平衡二叉树（AVL树）：| 左子树树高 - 右子树树高 | <= 1
* 最小失衡树：平衡二叉树插入新结点导致失衡的子树：调整：
    * LL型：根的左孩子右旋
    * RR型：根的右孩子左旋
    * LR型：根的左孩子左旋，再右旋
    * RL型：右孩子的左子树，先右旋，再左旋

### 其他树及森林

#### 树的存储结构

* 双亲表示法
* 双亲孩子表示法
* 孩子兄弟表示法

#### 并查集

一种不相交的子集所构成的集合 S = {S1, S2, ..., Sn}

#### 平衡二叉树（AVL树）

##### 性质

* | 左子树树高 - 右子树树高 | <= 1
* 平衡二叉树必定是二叉搜索树，反之则不一定
* 最小二叉平衡树的节点的公式：`F(n)=F(n-1)+F(n-2)+1` （1 是根节点，F(n-1) 是左子树的节点数量，F(n-2) 是右子树的节点数量）

平衡二叉树图片

![](https://gitee.com/huihut/interview/raw/master/images/Self-balancingBinarySearchTree.png)

##### 最小失衡树

平衡二叉树插入新结点导致失衡的子树

调整：

* LL 型：根的左孩子右旋
* RR 型：根的右孩子左旋
* LR 型：根的左孩子左旋，再右旋
* RL 型：右孩子的左子树，先右旋，再左旋

#### 红黑树

[RedBlackTree.cpp](DataStructure/RedBlackTree.cpp)

##### 红黑树的特征是什么？

1. 节点是红色或黑色。
2. 根是黑色。
3. 所有叶子都是黑色（叶子是 NIL 节点）。
4. 每个红色节点必须有两个黑色的子节点。（从每个叶子到根的所有路径上不能有两个连续的红色节点。）（新增节点的父节点必须相同）
5. 从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点。（新增节点必须为红）

##### 调整

1. 变色
2. 左旋
3. 右旋

##### 应用

* 关联数组：如 STL 中的 map、set

##### 红黑树、B 树、B+ 树的区别？

* 红黑树的深度比较大，而 B 树和 B+ 树的深度则相对要小一些
* B+ 树则将数据都保存在叶子节点，同时通过链表的形式将他们连接在一起。

#### B 树（B-tree）、B+ 树（B+-tree）

B 树、B+ 树图片

![B 树（B-tree）、B+ 树（B+-tree）](https://i.stack.imgur.com/l6UyF.png)

##### 特点

* 一般化的二叉查找树（binary search tree）
* “矮胖”，内部（非叶子）节点可以拥有可变数量的子节点（数量范围预先定义好）

##### 应用

* 大部分文件系统、数据库系统都采用B树、B+树作为索引结构

##### 区别

* B+树中只有叶子节点会带有指向记录的指针（ROWID），而B树则所有节点都带有，在内部节点出现的索引项不会再出现在叶子节点中。
* B+树中所有叶子节点都是通过指针连接在一起，而B树不会。

##### B树的优点

对于在内部节点的数据，可直接得到，不必根据叶子节点来定位。

##### B+树的优点

* 非叶子节点不会带上 ROWID，这样，一个块中可以容纳更多的索引项，一是可以降低树的高度。二是一个内部节点可以定位更多的叶子节点。
* 叶子节点之间通过指针来连接，范围扫描将十分简单，而对于B树来说，则需要在叶子节点和内部节点不停的往返移动。

> B 树、B+ 树区别来自：[differences-between-b-trees-and-b-trees](https://stackoverflow.com/questions/870218/differences-between-b-trees-and-b-trees)、[B树和B+树的区别](https://www.cnblogs.com/ivictor/p/5849061.html)

#### 八叉树

八叉树图片

![](https://upload.wikimedia.org/wikipedia/commons/thumb/3/35/Octree2.png/400px-Octree2.png)

八叉树（octree），或称八元树，是一种用于描述三维空间（划分空间）的树状数据结构。八叉树的每个节点表示一个正方体的体积元素，每个节点有八个子节点，这八个子节点所表示的体积元素加在一起就等于父节点的体积。一般中心点作为节点的分叉中心。

##### 用途

* 三维计算机图形
* 最邻近搜索

<a id="algorithm"></a>

## ⚡️ 算法

### 排序

排序算法 | 平均时间复杂度 | 最差时间复杂度 | 空间复杂度 | 数据对象稳定性
---|---|---|---|---
[冒泡排序](Algorithm/BubbleSort.h) | O(n<sup>2</sup>)|O(n<sup>2</sup>)|O(1)|稳定
[选择排序](Algorithm/SelectionSort.h) | O(n<sup>2</sup>)|O(n<sup>2</sup>)|O(1)|数组不稳定、链表稳定
[插入排序](Algorithm/InsertSort.h) | O(n<sup>2</sup>)|O(n<sup>2</sup>)|O(1)|稳定
[快速排序](Algorithm/QuickSort.h) | O(n*log<sub>2</sub>n) |  O(n<sup>2</sup>) | O(log<sub>2</sub>n) | 不稳定
[堆排序](Algorithm/HeapSort.cpp) | O(n*log<sub>2</sub>n)|O(n*log<sub>2</sub>n)|O(1)|不稳定
[归并排序](Algorithm/MergeSort.h) | O(n*log<sub>2</sub>n) | O(n*log<sub>2</sub>n)|O(n)|稳定
[希尔排序](Algorithm/ShellSort.h) | O(n*log<sup>2</sup>n)|O(n<sup>2</sup>)|O(1)|不稳定
[计数排序](Algorithm/CountSort.cpp) | O(n+m)|O(n+m)|O(n+m)|稳定
[桶排序](Algorithm/BucketSort.cpp) | O(n)|O(n)|O(m)|稳定
[基数排序](Algorithm/RadixSort.h) | O(k*n)|O(n<sup>2</sup>)| |稳定

> * 均按从小到大排列
> * k：代表数值中的 “数位” 个数
> * n：代表数据规模
> * m：代表数据的最大值减最小值
> * 来自：[wikipedia . 排序算法](https://zh.wikipedia.org/wiki/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95)

#### QuickSort
```C++
//LUG版//注意LUG`和DUP的优点，没想到最开始的版本还有优点。。
template <class RandomIt>
RandomIt partition(RandomIt first,RandomIt last){       //[first, last) //为了避免赋值还是要使用迭代器
    swap(*first, *(first + rand() % (last - first)) );  //注意这步优化点
    auto pivot = *first;
    --last;
    while(first<last){
        while(first<last && *last>=pivot)--last;        //都取等号，减少交换次数
        *first = *last;                                 //实际上不需要swap(*first,*last);
        while(first<last && *first<=pivot)++first;
        *last = *first;                                 //直接这样就行，循环内不用再做++first,--last;无非再判断一遍,但是就借用了first<last的判断
    }
    *first = pivot;
    return first;
}

//LGU版:关键在于怎么返回轴点位置，直接学习
//原来如此，直接保存在first里，最后归位即可
template <class RandomIt>
RandomIt partition_(RandomIt first, RandomIt last){     //[first, last)
    auto pivot = *first;
    auto mi = first;                                    //指向L的最后一个元素
    for(auto iter = first+1; iter<last; ++iter)
        if(*iter<pivot)
	    swap(*(++mi),*iter);                        //这里不得不交换了，因为是在滚动
    swap(*first, *mi);
    return mi;
}

template <class RandomIt> //如何保证来着
void qsort(RandomIt first,RandomIt last){
    if(last - first < 2) return;                  //if(first==last||next(first,1)==last)return;
    auto iter = partition_(first, last);
    qsort(first,iter);                            //[)前开后闭
    qsort(iter+1,last);                           //这里一定得缩小区间，去掉iter本身，否则会出不来
}
```

含有大量重复元素(Duplicate keys)时，采用`3-way partitions`,代码和思路见下，注意初始化

参见[链接](https://blog.csdn.net/Peggy_Chang/article/details/79451565)

```java
/*  -------------------------
 *  | < v| = v |\\\\\\| > v |
 *  -------------------------
 *        ↑lt   ↑i   ↑gt
**/
private static void sort(Comparable[] a, int lo, int hi)
{
    if (hi <= lo) return;
    int lt = lo, gt = hi;
    Comparable v = a[lo];
    int i = lo;
    while(i <= gt)
    {
        int cmp = a[i].compareTo(v);
        if(cmp < 0)
            swap(a[lt++], a[i++]);
        else if(cmp > 0) 
            swap(a[i], a[gt--]);
        else i++;
}

sort(a, lo, lt - 1);
sort(a, gt + 1, hi);
}
```

#### 堆排序
```C++
/* 大顶堆
 * usage: 
 * heap nheap({7,6,5,4,3,2,1});
 * nheap.heapify()/pop()/heapSort();
 */
class heap{
    vector<int> m_vec;

    /*      0
     *     1 2
     *    34 56
     *   parent = (idx-1)/2
     */
    int getParent(int idx){
    	return (idx-1)>>1;
    }

    /*
     * 将最后一个元素上滤; 用交替赋值代替交换
     */
    void upPercolation(int idx){
        if(idx<1)return;
        int val = m_vec[idx];
        while(idx>0 && m_vec[getParent(idx)] < val){
            m_vec[idx] = m_vec[getParent(idx)];
            idx = getParent(idx);
        }
        m_vec[idx] = val;
    }


    /*
     * 需要取最小/最大的孩子做置换
     * ProperParent(): 在n的索引范围里对节点idx及其子节点中选出可为Parent的节点
     */
    int ProperParent(int idx, int n, int val){
        int rchild = (idx+1)*2; //idx*2-1;  注意这俩不对
    	int lchild = rchild-1;  //idx*3;
        if(rchild<n){
	        int biggerIdx = m_vec[lchild] > m_vec[rchild]? lchild : rchild;
	        return m_vec[biggerIdx] >  val ? biggerIdx : idx;
	    }else if(lchild<n){
	        return m_vec[lchild] > val ? lchild : idx;
	    }else{
	        return idx; 
        }
    }
   
    /* 
     * 重新定义接口与实现如下, 实现简洁高效
     * 定义为将索引为idx的节点进行下滤：在建堆时需要指定idx，在堆排序时需要限定n
     */
    void downPercolation(int idx, int n){
        int pIdx;                                      //idx与其孩子中堪为父者
        int val = m_vec[idx];
        while( idx != (pIdx = ProperParent(idx, n, val))){    //当idx子节点更大时
            m_vec[idx] = m_vec[pIdx]; idx = pIdx;   
        }
        m_vec[idx] = val;
    }

public:
    template <class T>
    heap(const T& A){
        m_vec.assign(A.begin(), A.end());
    }

    void heapify_(){				//该实现即自上而下的上滤，复杂度O(nlogn)，不可接受，完全可以排序了
        int size = m_vec.size();
        int i=1;
        while(i<size){
            upPercolation(i);
            ++i;
        }		
    }

    void heapify(){				//转为自下而上的下滤，渐进复杂度O(n)
        int size = m_vec.size();
        for(int i = size/2 - 1; 0<=i; --i){
            downPercolation(i, size);
        }
    }    

    /*
     * make sure that m_vec is not empty();
     * */
    int pop(){
        int size = m_vec.size();
        if(size==0){
           // throw exception();
        }   
	    int ret = m_vec[0];
        m_vec[0]=m_vec[--size];	//删除堆顶，代之以末元素
	    m_vec.pop_back();
	    downPercolation(0, size);
	    return ret;
    }
    void display(){
        for(auto val: m_vec){
            cout<< val<< " ";
	    }
    }

    /*
     * 本地排序
     */
    void heapSort(){
        int n = m_vec.size();
        heapify();                     //先建堆
        while(n){
            swap(m_vec[0],m_vec[--n]);
            downPercolation(0,n);
        }
    }
};
```

### 查找

查找算法 | 平均时间复杂度 | 空间复杂度 | 查找条件
---|---|---|---
[顺序查找](Algorithm/SequentialSearch.h) | O(n) | O(1) | 无序或有序
[二分查找（折半查找）](Algorithm/BinarySearch.h) | O(log<sub>2</sub>n)| O(1) | 有序
[插值查找](Algorithm/InsertionSearch.h) | O(log<sub>2</sub>(log<sub>2</sub>n)) | O(1) | 有序
[斐波那契查找](Algorithm/FibonacciSearch.cpp) | O(log<sub>2</sub>n) | O(1) | 有序
[哈希查找](DataStructure/HashTable.cpp) | O(1) | O(n) | 无序或有序
[二叉查找树（二叉搜索树查找）](Algorithm/BSTSearch.h) |O(log<sub>2</sub>n) |   | 
[红黑树](DataStructure/RedBlackTree.cpp) |O(log<sub>2</sub>n) | |
2-3树 | O(log<sub>2</sub>n - log<sub>3</sub>n) |   | 
B树/B+树 |O(log<sub>2</sub>n) |   | 

### 图搜索算法

图搜索算法 |数据结构| 遍历时间复杂度 | 空间复杂度
---|---|---|---
[BFS广度优先搜索](https://zh.wikipedia.org/wiki/%E5%B9%BF%E5%BA%A6%E4%BC%98%E5%85%88%E6%90%9C%E7%B4%A2)|邻接矩阵<br/>邻接链表|O(\|v\|<sup>2</sup>)<br/>O(\|v\|+\|E\|)|O(\|v\|<sup>2</sup>)<br/>O(\|v\|+\|E\|)
[DFS深度优先搜索](https://zh.wikipedia.org/wiki/%E6%B7%B1%E5%BA%A6%E4%BC%98%E5%85%88%E6%90%9C%E7%B4%A2)|邻接矩阵<br/>邻接链表|O(\|v\|<sup>2</sup>)<br/>O(\|v\|+\|E\|)|O(\|v\|<sup>2</sup>)<br/>O(\|v\|+\|E\|)

注意：BFS和DFS的递推形式总结 `@todo`



### 其他算法

算法 |思想| 应用
---|---|---
[分治法](https://zh.wikipedia.org/wiki/%E5%88%86%E6%B2%BB%E6%B3%95)|把一个复杂的问题分成两个或更多的相同或相似的子问题，直到最后子问题可以简单的直接求解，原问题的解即子问题的解的合并|[循环赛日程安排问题](https://github.com/huihut/interview/tree/master/Problems/RoundRobinProblem)、排序算法（快速排序、归并排序）
[动态规划](https://zh.wikipedia.org/wiki/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92)|通过把原问题分解为相对简单的子问题的方式求解复杂问题的方法，适用于有重叠子问题和最优子结构性质的问题|[背包问题](https://github.com/huihut/interview/tree/master/Problems/KnapsackProblem)、斐波那契数列
[贪心法](https://zh.wikipedia.org/wiki/%E8%B4%AA%E5%BF%83%E6%B3%95)|一种在每一步选择中都采取在当前状态下最好或最优（即最有利）的选择，从而希望导致结果是最好或最优的算法|旅行推销员问题（最短路径问题）、最小生成树、哈夫曼编码



### 串
1. 字符串匹配(参考[文章](https://www.zhihu.com/question/21923021))
Brute-Force 的复杂度是 O(nm)的，对此做改进，由于实际很难降低字符串比较的复杂度，所以应该尝试降低比较的趟数，尽可能利用残余信息。
每一次失败的匹配实际都能带来一些有用信息，如主串的某一个子串等于模式串的某一个前缀。
![failure_trial](https://pic1.zhimg.com/80/v2-7dc61b0836af61e302d9474eeeecfe83_720w.jpg?source=1940ef5c)

使用next数组作为标尺，这里记录了最长前后相等的串

![correspondence](https://pic2.zhimg.com/80/v2-6ddb50d021e9fa660b5add8ea225383a_720w.jpg?source=1940ef5c)

e.g.最后个a失效后[ababa]移动2


或称之为部分匹配表(Partial Match Table)： PMT中的值是字符串的前缀集合与后缀集合的交集中最长元素的长度。(不包含字符串本身)

#### Karp-Rabin算法: 串即是数
需要注意溢出问题，有几种思路
1. 可尝试`unsigned long long`等
2. 取模，则会引入哈希碰撞，可采用如下方式改进
   - 双哈希，即采用不同进制和模数，类似布隆过滤器
   - 拉链法，存储存储每个key对应的head,length,即`unordered_map<nt, list<pair>>`

```C++
    void getHash(const string& s,int idx, int& hashcode){
        int digit;
        if(idx==0){
            for(int i=0;i<10;++i)
                hashcode = (hashcode * base + getDigit(s[i]));
            return;
        }
        hashcode = (hashcode - getDigit(s[idx-1])*topWeight);
        //if(hashcode<0)hashcode+=mod;
        hashcode = (hashcode * base + getDigit(s[idx+9]));
    }
```

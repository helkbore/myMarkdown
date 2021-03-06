# java堆栈
JAVA在程序运行时，在内存中划分5片空间进行数据的存储。分别是：
1. 寄存器
2. 本地方法区
3. 方法区
4. 栈
5. 堆

## 通用编译原理上的内存分配策略
1. 静态
2. 栈式
3. 堆式

### 静态
编译时就确定存储空间的需求, 因此不允许有可变的无法准确计算存储空间的结构出现(可变数组, 嵌套或递归结构)

### 栈式
+ 动态存储分配, 但是在运行中进入一个程序模块时,必须知道该程序模块所需的数据区大小才能够为其分配内存
+ 用来执行程序的
+ c/c++所有方法调用都是用栈, 通过指针, 速度最快
+ 数据结构: 先进后出
+ 操作系统: 由操作系统自动分配释放 ，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈
+ 缓存方式: 栈使用的是一级缓存， 他们通常都是被调用时处于存储空间中，调用完毕立即释放。
+ 申请影响: 只要栈的剩余空间大于所申请空间，系统将为程序提供内存，否则将报异常提示栈溢出。
+ 内存: 有栈顶地址和最大容量的连续的内存空间, 向低地址扩展

### 堆式
+ 堆式存储分配则专门负责在编译时或运行时模块入口处都无法确定存储要求的数据结构的内存分配, 堆中的内存可以按照任意顺序分配和释放.
+ 用来放对象的
+ 堆是应用程序在运行的时候请求操作系统分配给自己内存，由于从操作系统管理的内存分配,所以在分配和销毁时都要占用时间
+ 面向对象的多态性, 堆是必不可少的.
+ 数据结构: 队列优先, 先进先出/ 堆可以被看成是一棵树，如：堆排序。
+ 操作系统: 一般由程序员分配释放， 若程序员不释放，程序结束时可能由OS回收，分配方式倒是类似于链表。
+ 缓存方式: 堆则是存放在二级缓存中，生命周期由虚拟机的垃圾回收算法来决定（并不是一旦成为孤儿对象就能被回收）。所以调用这些对象的速度要相对来得低一些。
+ 申请影响: 操作系统有一个空闲的内存地址链表, 当程序申请时, 会寻找第一个空间大于申请空间的堆节点分配给程序, 系统会将多余那部分重新放入空闲链表里
+ 内存: 不连续的, 遍历是由低地址向高地址扩展

## JVM中的堆栈
+ JVM为每个新创建的线程都分配一个堆栈
+ 堆栈以帧为单位保存线程的状态。
+ JVM对堆栈只进行两种操作:以帧为单位的压栈和出栈操作。


## 栈(stack)
+ 基础数据类型
+ 局部变量
+ 对象的引用变量
+ 特点: 执行完毕立即释放

## 堆(heap)
new创建的实例化对象及数组，是存放在堆内存中的，用完之后靠垃圾回收机制不定期自动消除。

+ new创建的对象和数组。
+ 堆内存中所有的实体都有内存地址值。
+ 堆内存中的实体是用来封装数据的，这些数据都有默认初始化值。
+ 堆内存中的实体不再被指向时，JVM启动垃圾回收机制，自动清除，这也是JAVA优于C++的表现之一（C++中需要程序员手动清除）。

## 实例
Car c = new Car(); // 将地址给了c
Car c2 = c // c 和 c2 都指向同一个 new Car() 内存地址


## 参考
https://www.cnblogs.com/ibelieve618/p/6380328.html
http://blog.csdn.net/jerryao/article/details/874101

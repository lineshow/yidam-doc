# JVM
## 内存控制
### 垃圾清理空间回收
#### 知识点
- GC算法
    这种假设是错误的。PS-Scavenge 将用于年轻一代(eden，幸存者)和PS-Marksweep将用于老年代。唯一的“重叠”是PS-Scavenge将对象移动到老年代，一旦他们已经存在一段时间，让PS-MarkSweep处理他们。

    为不同的池使用不同的垃圾收集器的好处是，对于eden池中的对象有效的算法不一定适用于老年代对象。

    本文介绍了不同垃圾收集器协同工作的各种选项。
    [This article covers the various options for different garbage collectors working together](http://www.fasterj.com/articles/oraclecollectors1.shtml)
    当没有空间将对象移动到老年代时，就会出现算法无差别收集，Sun的这篇(确实很老)白皮书中说:
    [this (admittedly old) whitepaper from Sun](https://www.oracle.com/technetwork/java/javase/memorymanagement-whitepaper-150215.pdf)

        ...the young generation collection algorithm is not run. Instead, the old generation collection algorithm is used on the entire heap.


### 线程
#### 知识点

- 局部变量区: java栈帧的局部变量区被组织为一个以字长为单位，从0开始计数的数组。字节码指令通过从0开始的索引来使用其中的数据。类型为int，float，**reference**和returnaddress的值在数组中只占一项，而类型为byte，short和char的值在存入数组前都**将被转换为int**。但是long和double类型在数组中却**占据连续的两**项。
- 操作数栈也称为操作栈，是一个后入先出的数据结构，同局部变量表一样，操作数栈的深度也是在编译阶段确定，并且存储在Code属性的max_stacks数据项中，操作数栈中的数据可以是任意类型的，**32位类型的数据占据一个栈容量，64位类型的数据占据两个栈容量**。

    操作数栈的使用过程包括以下几项：
    1)栈帧刚创建时，操作数栈是空的；
    2)虚拟机可以对操作数栈进行入栈操作，比如把局部变量表或方法参数中的数据入栈；
    3)虚拟机也可以对操作数栈进行出栈操作；
    4)向其他方法传递的参数，也存在操作数栈中；
    5)调用其他方法的返回结果，也存储在操作数栈中。


# 并发
## 线程
### threadlocal
#### 知识点

​	![一图千言](/Users/edgarchan/Documents/my_home/1555470787073.jpg)

- [ThreadLocal源码分析](https://github.com/CL0610/Java-concurrency/blob/master/18.%E4%B8%80%E7%AF%87%E6%96%87%E7%AB%A0%EF%BC%8C%E4%BB%8E%E6%BA%90%E7%A0%81%E6%B7%B1%E5%85%A5%E8%AF%A6%E8%A7%A3ThreadLocal%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E9%97%AE%E9%A2%98/%E4%B8%80%E7%AF%87%E6%96%87%E7%AB%A0%EF%BC%8C%E4%BB%8E%E6%BA%90%E7%A0%81%E6%B7%B1%E5%85%A5%E8%AF%A6%E8%A7%A3ThreadLocal%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E9%97%AE%E9%A2%98.md)

    
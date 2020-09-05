# 计算机复试面试题总结

<https://blog.csdn.net/liupeng19970119/article/details/88316368>

**面试问题之编程语言**

1。C++的特点是什么？

封装，继承，多态。支持面向对象和面向过程的开发。

2.C++的异常处理机制？

抛出异常和捕捉异常进行处理。（实际开发）

3.c和c++，java的区别?

c是纯过程，c++是对象加过程，java是纯面向对象的

4.纯虚函数？

被virtual修饰的成员函数，再基类不能实现，而他的实现放到派生类中实现。

5.什么是内存泄漏？

没有delete

6.java怎么处理对象分配和释放的？

java把内存分为堆栈空间存储，在堆中new的空间不用自己收回，自动垃圾收回。

7.java的特点？

一次编译到处运行，没有指针，完全对象化。

8.c++和c中字符串区别？

c++是类，c中是基本类型函数。

**面试问题之数据结构**

![img](https://img-blog.csdnimg.cn/20190315143432196.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdXBlbmcxOTk3MDExOQ==,size_16,color_FFFFFF,t_70)

1.顺序结构和链式结构的区别？

顺序结构是指内存连续的存储单元进行存储，而链式结构是指 内存不连续的结构，通过一个节点指向另外一个节点的地址。

2.栈和队列的区别？

栈是先进后出的特殊线性表，队列是先进先出的线性表。

3.复杂度是什么？

复杂度包括时间复杂度和空间复杂度，用来评价一个算法的好坏。

4.头节点的作用是什么？

头节点是指向初始地址的一个节点，它本身数据段没有内容，通过它可以标识这个链表。

5介绍以下各种树

树，二叉树：有左右子树的区分和度不超过2.

二叉排序树：左子树均小于根，根均小于右节点。。

线索二叉树：设置两个标识标记左右指针指向的是孩子还是前躯节点。

平衡二叉树：左右子树高度差绝对值小于等于1。

哈夫曼树：压缩用的。权值大小排列。

完全二叉树：只能从右边为空。

6.度为2的树和二叉树的区别：

二叉树有左右子树的定义。

7..树的存储结构

孩子链存储结构和双亲存储结构。

8.树的遍历

先序中序后序三种。递归实现。

9.图的存储

邻接矩阵和邻接表，是多对多的关系，分为有向图和无向图。

![img](https://img-blog.csdnimg.cn/20190315151231892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdXBlbmcxOTk3MDExOQ==,size_16,color_FFFFFF,t_70)

10线性表.查找有那几类？

直接查找和有序表的二分查找。

10.排序算法的介绍？

插入排序有直接插入和折半插入。都是在有序表里插入进去的。

交换排序：冒泡，快速：以一个数字划分两个区域，然后分别对两个区域继续划分，直到区间为一。注意快排是不稳定。

选择排序：简单的选择排序，堆排序

归并排序：将两个有序表归并到一个有序表。将两个有序表放到一起进行各个比较，比较完之后放回原来数组内。

11。什么是稳定的算法？

不乱动已经排序好的数字，这样算法稳定一些。

 

**面试问题之网络**

![img](https://img-blog.csdnimg.cn/20190316190539304.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdXBlbmcxOTk3MDExOQ==,size_16,color_FFFFFF,t_70)

1.谈谈对TCP/IP协议的理解

 有5层结构，分层处理。

![img](https://img-blog.csdnimg.cn/20190315155655838.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdXBlbmcxOTk3MDExOQ==,size_16,color_FFFFFF,t_70)

2。TCP/UPD的区别？

TCP是可靠的面向连接的传输控制协议，UDP是不可靠的无连接传输数据报文协议。

 

![img](https://img-blog.csdnimg.cn/2019031515573843.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdXBlbmcxOTk3MDExOQ==,size_16,color_FFFFFF,t_70)

 

3.IP和mac的区别？

IP是网络层，MAC是数据链路层且地址是全球唯一的。

4.登陆baidu.com,简述协议过程

ARRP(获得网关地址) - DNS（获得IP地址） - TCP

5.简述ARRP的作用

![img](https://img-blog.csdnimg.cn/20190315165239752.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdXBlbmcxOTk3MDExOQ==,size_16,color_FFFFFF,t_70)

6.hub,switch,router属于OSI哪一层？

hub是集线器属于物理层，交换机是数据链路层，router是路由器网络层的，负责不同网络结合。

7.子网掩码和IP地址怎么理解？

​    在国际互联网(Internet)上有成千百万台主机（host），为了区分这些主机，人们给每台主机都分配了一个专门的“地址”作为标识，称为IP地址。子网掩码的作用是用来区分网络上的主机是否在同一网络段内。子网掩码不能单独存在，它必须结合IP地址一起使用。子网掩码只有一个作用，就是将某个IP地址划分成网络地址和主机地址两部分。

8.ipv4,ipv6的区别？

IPV6更安全，更大的存储空间。

9.XML和HTML区别?

跨平台的标记语言，重在储存数据。HTML重在存储界面显示内容

**面试问题之操作系统**

**1.进程和线程的区别？**

![img](https://img-blog.csdnimg.cn/20190315165226309.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdXBlbmcxOTk3MDExOQ==,size_16,color_FFFFFF,t_70)

2.进程调度方式

时间片轮转和先来先服务。进程有三种状态阻塞就绪，运行。

3.死锁问题？

![img](https://img-blog.csdnimg.cn/20190315165318297.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdXBlbmcxOTk3MDExOQ==,size_16,color_FFFFFF,t_70)

产生原因：资源竞争 必要条件：互斥，请求与保持，不剥夺，环路等待。处理方法：撤销进程或者剥夺资源。代表性算法：英航家算法。

4.虚拟存储的意义和方法？

根据程序执行的互斥性和空间与时间局域性两个特点，允许作业装入时候只装入一部分，另一部分存放在磁盘上，调用时候将常用的放入内存，其他暂时不用的放入外存中。这样一个小的主存空间也可以运行一个比它大的作业。常用的虚拟存储技术有分页分段存储管理。

5.windows和linux使用的文件系统？

window:fat32.linux:ext2,fat32.

6.中断是怎么操作的？

中断请求-中断相应-断点保护---执行中断服务程序---断点恢复---中断返回

 

**面试问题之计算机组成原理**

1.什么是冯诺伊曼结构？

输入输出，计算单元，控制单元，存储单元。

2.高速缓存的作用

连接CPU和内存。

3。cache和寄存器区别？

寄存器是暂时存储的CPU组成部分，cache用来做高度CPU和低速的主存之间加速带。

4.指令系统

CISC复杂指令集，RISC是精简指令集。

5流水线

将重复性的过程分为若干个子过程来完成。

6总线和I/O

总线是指数据通信的连接线，有地址，数据，控制指令。

I/O的方式有程序性，中断性，通道,DMA。

 

**面试问题之数据库**

1.范式的定义？

改造关系模式，通过分解关系模型来消除其中不合适的数据依赖，以决绝插入异常，删除异常，数据用余。

2.事务？

类似查询一次的命令，要求全部执行完。

3.事务执行的四个基本要素？

原子性，一致性，隔离性，持久性。

4.数据库和文件系统的比较？

数据库结构化，共享性好，独立性。有界面接口。

5.数据模型有哪几种？

关系模型，层次模型，网状模型

6索引建的多的好还是少的好？

恰当把握，多的话占空间，少的话查询不足，速度达不到。

 

**面试问题之软件工程题**

1.软件重用？

同一个函数重复用。

2.软件测试类型？

单元，集成，黑盒，白盒。

3.软件工程步骤

需求，设计，开发，测试。

4.UML

系统流程建模工具

5软件工程的认识？

运用工程化的方法管理软件开发。
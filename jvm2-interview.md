[TOC]

# jvm线上调优

<https://www.cnblogs.com/zhuyeshen/p/10997632.html>

# [linux 下使用命令查看jvm信息](https://www.cnblogs.com/zhuyeshen/p/10997632.html)

<https://blog.csdn.net/weixin_43810394/article/details/93418224>

java程序员除了编写业务代码之外，特别是项目上线之后，更需要关注的是系统的性能表现，这个时候就需要了解一下jvm的性能表现了，可以借助于java虚拟机自带的一些分析工具，主要有三个常用的命令。

\1. jmap

​    这个命令是用来查看当前系统中jvm进程 heap dump的情况，包括对象的数量，对象所占内存的大小

​    使用方式：先使用jps查看进程id

​    ![img](https://img2018.cnblogs.com/blog/1358633/201906/1358633-20190610145108889-631270384.png)

 



​    使用 jmap -dump:live,file=b.map 22467 将live进程生成java堆转储快照
![img](https://img2018.cnblogs.com/blog/1358633/201906/1358633-20190610145115946-111358415.png)

 


   

​    使用 jmap -heap PID 生成java堆的详细信息

​    使用 jmap -histo PID 生成java堆中对象的相关信息，包含数量以及占用的空间大小

​    ![img](https://img2018.cnblogs.com/blog/1358633/201906/1358633-20190610145123906-1011483461.png)

 



\2. jstat

   主要是用来监控 heap size 和 jvm垃圾回收情况，尤其是gc情况的监控，如果老年代发生full gc，那么很可能会导致内存泄漏的可能性

![img](https://img2018.cnblogs.com/blog/1358633/201906/1358633-20190610145134657-1578491649.png)

 



可以看到新生代survivor S0, survivor S1 heap上的空间 使用百分比，堆中新生代Eden 的空间使用百分比，老年代Old 空间的使用百分比，内存的使用百分比，新生代Yong gc 的统计次数，新生代gc 花费的时间,full gc 的次数，花费的时间，当前进程总的gc时间，这里要注意一点，full gc很具有代表性，full gc次数 和时间 指标很能显示系统性能问题，这两个指标很大，很大程度上说明了程序中有问题，垃圾一直回收不掉

\3. jstack

   先使用 top 查看系统中消耗cpu比较多的进程，然后使用 top -p PID -H来查看当前进程中比较消耗cpu的线程，拿到消耗cpu比较高的线程pid,先转换成16进制的，最后使用jstack pid|grep 16进制的线程id

![img](https://img2018.cnblogs.com/blog/1358633/201906/1358633-20190610145143419-2068695733.png)

 



jstack -pid可以用来分析进程情况

![img](https://img2018.cnblogs.com/blog/1358633/201906/1358633-20190610145149714-666176750.png)

 



<https://www.cnblogs.com/williamjie/p/9329776.html>

# [Linux使用jstat命令查看jvm的GC情况](https://www.cnblogs.com/williamjie/p/9329776.html)



## 命令格式

## jstat命令命令格式：

jstat [Options] vmid [interval] [count]

## 参数说明：

Options，选项，我们一般使用 -gcutil 查看gc情况
vmid，VM的进程号，即当前运行的java进程号
interval，间隔时间，单位为秒或者毫秒
count，打印次数，如果缺省则打印无数次

## 示例说明

## 示例

通常运行命令如下：

**jstat -gc 30996 3000**

**即：每3秒一次显示进程号为30996的java进程的GC情况**

**或使用命令：jstat -gcutil 30996 3000**

![img](file:///C:/Users/qigw/AppData/Local/Temp/enhtmlclip/Image.png)



 

或者使用如下命令：

![img](file:///C:/Users/qigw/AppData/Local/Temp/enhtmlclip/Image(1).png)

 ![img](https://images2015.cnblogs.com/blog/1019110/201609/1019110-20160901153203730-121544986.png)

## 结果说明

显示内容说明如下（部分结果是通过其他其他参数显示的，暂不说明）：

 

​         S0C：年轻代中第一个survivor（幸存区）的容量 (字节) 
​         S1C：年轻代中第二个survivor（幸存区）的容量 (字节) 
​         S0U：年轻代中第一个survivor（幸存区）目前已使用空间 (字节) 
​         S1U：年轻代中第二个survivor（幸存区）目前已使用空间 (字节) 
​         EC：年轻代中Eden（伊甸园）的容量 (字节) 
​         EU：年轻代中Eden（伊甸园）目前已使用空间 (字节) 
​         OC：Old代的容量 (字节) 
​         OU：Old代目前已使用空间 (字节) 
​         PC：Perm(持久代)的容量 (字节) 
​         PU：Perm(持久代)目前已使用空间 (字节) 
​         YGC：从应用程序启动到采样时年轻代中gc次数 
​         YGCT：从应用程序启动到采样时年轻代中gc所用时间(s) 
​         FGC：从应用程序启动到采样时old代(全gc)gc次数 
​         FGCT：从应用程序启动到采样时old代(全gc)gc所用时间(s) 
​         GCT：从应用程序启动到采样时gc用的总时间(s) 
​         NGCMN：年轻代(young)中初始化(最小)的大小 (字节) 
​         NGCMX：年轻代(young)的最大容量 (字节) 
​         NGC：年轻代(young)中当前的容量 (字节) 
​         OGCMN：old代中初始化(最小)的大小 (字节) 
​         OGCMX：old代的最大容量 (字节) 
​         OGC：old代当前新生成的容量 (字节) 
​         PGCMN：perm代中初始化(最小)的大小 (字节) 
​         PGCMX：perm代的最大容量 (字节)   
​         PGC：perm代当前新生成的容量 (字节) 
​         S0：年轻代中第一个survivor（幸存区）已使用的占当前容量百分比 
​         S1：年轻代中第二个survivor（幸存区）已使用的占当前容量百分比 
​         E：年轻代中Eden（伊甸园）已使用的占当前容量百分比 
​         O：old代已使用的占当前容量百分比 
​         P：perm代已使用的占当前容量百分比 
​         S0CMX：年轻代中第一个survivor（幸存区）的最大容量 (字节) 
​         S1CMX ：年轻代中第二个survivor（幸存区）的最大容量 (字节) 
​         ECMX：年轻代中Eden（伊甸园）的最大容量 (字节) 
​         DSS：当前需要survivor（幸存区）的容量 (字节)（Eden区已满） 
​         TT： 持有次数限制 

​         MTT ： 最大持有次数限制 

# jvm 性能调优工具之 jmap



<https://www.jianshu.com/p/a4ad53179df3>

# [java命令--jhat命令使用](https://www.cnblogs.com/baihuitestsoftware/articles/6406271.html)

<https://www.cnblogs.com/baihuitestsoftware/articles/6406271.html>
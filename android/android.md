## [android 开发题](https://juejin.cn/post/6844903891625050119)
## [android 进阶到高级-突破篇](https://pangrongxian.github.io/2017/07/17/Android%20%E8%BF%9B%E9%98%B6%E5%88%B0%E9%AB%98%E7%BA%A7%20-%20%E7%AA%81%E7%A0%B4%E7%AF%87/)

## dvm和jvm都是什么东西，有什么区别？
什么是dvm：Dalvik是Google公司自己设计用于Android平台的Java虚拟机,每一个Dalvik 应用作为一个独立的Linux 进程执行。独立的进程可以防止在虚拟机崩溃的时候所有程序都被关闭。

Dalvik  virtual machine 和Java virtual machine    （dvm和jvm）的区别

1、Dalvik主要是完成对象生命周期管理，堆栈管理，线程管理，安全和异常管理，以及垃圾回收等等重要功能  
2、Dalvik负责进程隔离和线程管理，每一个Android应用在底层都会对应一个独立的Dalvik虚拟机实例，其代码在虚拟机的解释下得以执行  
3、不同于Java虚拟机运行java字节码，Dalvik虚拟机运行的是其专有的文件格式Dex  
4、dex文件格式可以减少整体文件尺寸，提高I/O操作的类查找速度  
5、odex是为了在运行过程中进一步提高性能，对dex文件的进一步优化  
6、所有的Android应用的线程都对应一个Linux线程，虚拟机因而可以更多的依赖操作系统的线程调度和管理机制  
7、有一个特殊的虚拟机进程Zygote，他是虚拟机实例的孵化器。它在系统启动的时候就会产生，它会完成虚拟机的初始化，库的加载，预制类库和初始化的操作。如果系统需要一个新的虚拟机实例，它会迅速复制自身，以最快的数据提供给系统。对于一些只读的系统库，所有虚拟机实例都和Zygote共享一块内存区域。

**上面有7个回答，面试时只需要说明jvm dvm分别是什么东西，然后dvm负责的任务是什么，jvm负责的任务是什么即可。也就是说答出dvm jvm的概念，还有区别的前三点即可。当然如果你有能力说出下方的4567，面试效果会更好。**

## [热修复原理](https://juejin.cn/post/6844903985602789384)

## [Service AIDL之重连方法](https://www.jianshu.com/p/476abecc7292)

## [美团技术团队](https://tech.meituan.com/archives)

## [启智观-ActivityManagerService](https://duanqz.github.io/2016-07-15-AMS-LaunchProcess#Android%E5%90%AF%E6%99%BA%E8%A7%82)

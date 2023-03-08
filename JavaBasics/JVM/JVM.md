# JVM
  ## JVM的组成
  - 类装载器(Class loader)
  - 执行引擎(Execution engine)
  - 本地库接口(Native Interface)
  - 运行时数据区(Runtime data area)
   
  ## 运行时数据区
  - **堆**： 
    - 新生代-[10, 伊甸区eden-8,幸存区S0-1,幸存区S1-1]
    - 老年代-[20]
  - **元空间**
    - JDK1.8属于本地内存, 包括虚拟机加载的类信息和运行时常量池，存储原来的方法区PermGen，即永久代
  - **Java虚拟机栈**
  - **本地方法栈**
  - **程序计数器**
    - 包含线程执行指令的地址

## JVM常用参数
  - -Xms 和 -Xmx：这两个参数用于设置JVM的初始堆大小和最大堆大小。通过调整这些参数，可以控制应用程序可以使用的内存量。例如，-Xms512m -Xmx2g 可以将JVM的初始堆大小设置为512MB，最大堆大小设置为2GB。

  - -XX:PermSize 和 -XX:MaxPermSize：这两个参数用于设置永久代（Permanent Generation）的初始大小和最大大小。在JDK8及以后的版本中，永久代已经被移除，可以使用-XX:MetaspaceSize 和 -XX:MaxMetaspaceSize 参数来代替。

  - -XX:+UseConcMarkSweepGC：这个参数启用了CMS垃圾回收器（Concurrent Mark and Sweep），它可以在应用程序运行期间执行垃圾回收操作，减少应用程序的暂停时间。同时，这个参数也会增加CPU的使用率，因为垃圾回收器是在一个独立的线程中运行的。

  - -XX:+UseG1GC：这个参数启用了G1垃圾回收器（Garbage First），它是JDK7及以后版本中引入的一种新的垃圾回收器，可以提供更短的暂停时间和更高的吞吐量。

  - -XX:MaxGCPauseMillis：这个参数用于设置最大垃圾回收暂停时间，以毫秒为单位。如果应用程序需要快速响应，可以将这个参数设置为一个较小的值。

  - -XX:+PrintGCDetails 和 -XX:+PrintGCDateStamps：这两个参数可以打印垃圾回收器的详细信息和时间戳，以便分析垃圾回收器的性能。

-XX:+HeapDumpOnOutOfMemoryError：这个参数可以在应用程序发生内存溢出错误时生成堆转储文件，以便分析内存使用情况。

# Hotspot原理

--- 

- [回到首页](../../README.md)
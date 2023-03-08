# ConcurrentHashMap

 - ConcurrentHashMap在JDK 1.7中使用的数组 加 链表的结构，其中数组分为两类，大树组Segment 和 小数组 HashEntry，而加锁是通过给Segment添加ReentrantLock重入锁来保证线程安全的。
 - ConcurrentHashMap在JDK1.8中使用的是数组 加 链表 加 红黑树的方式实现，它是通过 CAS 或者 synchronized  来保证线程安全的，并且缩小了锁的粒度，查询性能也更高。
 - ConcurrentHashMap在源码中加入不允许插入 null （空） 值的设计，主要目的是为了防止并发场景下的歧义问题。
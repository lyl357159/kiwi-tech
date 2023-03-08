# HashMap

## HashMap的数据结构
   - jdk1.8以前：数组+链表
   - jdk1.8以后：数组+链表+红黑色
     > **HashMap中链表转为红黑树的条件**
     > 条件一：如果某一条链表的长度超过8（默认），即数组 arr[i] 处存放的链表长度大于8；
     > 条件二：数组长度大于等于64。
     > 满足以上两个条件，数组 arr[i] 处的链表将自动转化为红黑树，其他位置如 arr[i+1] 处的数组元素仍为链表，不受影响


- HashMap死循环只发生在JDK1.7版本中，主要原因是JDK1.7中的HashMap，在头插法 加 链表 加 多线程并发 加 扩容这几个情形累加到一起就会形成死循环。多线程环境下建议采用ConcurrentHashMap替代。在JDK1.8中，HashMap改成了尾插法，解决了链表死循环的问题。

## 参考资料
   - [关于HashMap的数据结构](https://blog.csdn.net/yaovirus/article/details/106083471)
   - [HashMap中链表转为红黑树的条件](https://blog.csdn.net/qq_26896085/article/details/115720888)
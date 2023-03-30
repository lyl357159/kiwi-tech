# Lock

## 产生死锁形成的四个条件
  |死锁条件|说明|避免死锁方法
  |---|---|---|
  |互斥条件|共享资源a和b只能被一个线程占用|无法破坏
  |请求和保持条件|线程T1巳经获取共享资源a，在等待共享资源b的时候，不释放共享资源a|首次执行一次性申请所有的资源
  |不可抢占条件|其它线程不能强行抢占线程T1占有的资源|占用部分资源的线程在进一步申请其他资源的时候如果申请不到，我们可以主动释放它占有的资源
  |循环等待|程T1等待线程T2占有的资源，线程T2等待线程T1占有的资源，这形成了循环等待|可以通过按序申请资源来预防死锁的产生。所谓按序申请，就是给资源编号，所有线程可以按照线性化的序号顺序去申请共享资源，先申请需要序号小的，再申请序号大的，这样循环等待自然就不存在了。

---
## synchronized和ReentrantLock的比较
  - 共同点
    - 都是用于控制多线程对共享对象的访问
    - 都是可重入锁
    - 都保证了可见性和互斥性
  - 不同点
    - ReentrantLock显示获取和释放锁；synchronized隐式获取和释放锁，为了避免程序出现异常无法释放锁，需要在实体ReentrantLock时必须在finally语句块中执行释放锁的操作。
    - ReentrantLock可响应中断、可轮回，为处理锁提供了更多的灵活性。
    - ReentrantLock是API级别的，synchronized是JVM级别的。
    - ReentrantLock可以定义公平锁。
    - synchronized底层是同步阻塞机制，采用的是悲观并发策略；   
    - ReentrantLock是同步非阻塞，采用的是乐观并发策略。
    - Lock是一个接口，而synchronized是Java中的关键字，synchronized是内置语言实现的。
    - 通过Lock可以知道有没有成功获取锁，通过synchronized是无法做到的。

## Synchronized和ReentrantLock的主要区别：
  - **实现方式**：Synchronized是Java语言内置的关键字，由JVM底层实现，而ReentrantLock是Java提供的类，在Java API中提供了对其的支持。
  - **锁的获取方式**：Synchronized是隐式锁，也就是说锁的获取和释放是由JVM隐式完成的，而ReentrantLock是显式锁，需要手动获取和释放锁。
  - **锁的粒度**：Synchronized是对代码块或方法进行加锁，而ReentrantLock可以根据需要对代码块或方法进行加锁或解锁。
  - **性能**：在低并发情况下，Synchronized的性能要优于ReentrantLock。但是，在高并发情况下，ReentrantLock的性能要优于Synchronized，因为ReentrantLock具有更好的扩展性和灵活性。
  > 
  总体来说，Synchronized是一种简单而有效的线程同步机制，适用于低并发情况下的线程同步。而ReentrantLock则是一种更加灵活和高级的线程同步机制，可以更好地应对高并发场景下的线程同步问题。但是，ReentrantLock的使用比Synchronized更加复杂，需要手动获取和释放锁，因此在实际应用中需要谨慎使用。


--- 
- [返回首页](../../../README.md)
# Hystrix
  - Hystrix 实现资源隔离，有两种策略：
    - 线程池隔离
    - 信号量隔离
  

  ## execution.isolation.strategy
- 指定了 HystrixCommand.run() 的资源隔离策略：THREAD or SEMAPHORE，一种基于线程池，一种基于信号量。
  
     ```java
        // to use thread isolation
        HystrixCommandProperties.Setter().withExecutionIsolationStrategy(ExecutionIsolationStrategy.THREAD)

        // to use semaphore isolation
        HystrixCommandProperties.Setter().withExecutionIsolationStrategy(ExecutionIsolationStrategy.SEMAPHORE)
     ```
    - 线程池机制，每个 command 运行在一个线程中，限流是通过线程池的大小来控制的；信号量机制，command 是运行在调用线程中（也就是 Tomcat 的线程池），通过信号量的容量来进行限流。
  
  - 默认的策略就是线程池。
    - 线程池其实最大的好处就是对于网络访问请求，如果有超时的话，可以避免调用线程阻塞住。
    - 而使用信号量的场景，通常是针对超大并发量的场景下，每个服务实例每秒都几百的 QPS，那么此时你用线程池的话，线程一般不会太多，可能撑不住那么高的并发，如果要撑住，可能要耗费大量的线程资源，那么就是用信号量，来进行限流保护。一般用信号量常见于那种基于纯内存的一些业务逻辑服务，而不涉及到任何网络访问请求。


## Hystrix 断路器
  - Hystrix 断路器有三种状态，分别是关闭（Closed）、打开（Open）与半开（Half-Open）
  - 断路器状态由 Close 转换到 Open，在之后 SleepWindowInMilliseconds 时间内，所有经过该断路器的请求会被断路，不调用后端服务，直接走 Fallback 降级机制，默认值 5000(ms)。而在该参数时间过后，断路器会变为 Half-Open 半开闭状态，尝试让一条请求经过断路器，看能不能正常调用。如果调用成功了，那么就自动恢复，断路器转为 Close 状态。

## 参考资料
   - [Java 工程师进阶知识完全扫盲](https://doocs.github.io/advanced-java/#/docs/high-availability/hystrix-execution-isolation)
# CAP
  - 在理论计算机科学中，CAP 定理（CAP theorem），又被称作布鲁尔定理（Brewer's theorem），它指出对于一个分布式计算系统来说，不可能同时满足以下三点：

    - 一致性（Consistency） （等同于所有节点访问同一份最新的数据副本）
    - 可用性（Availability）（每次请求都能获取到非错的响应——但是不保证获取的数据为最新数据）
    - 分区容错性（Partition tolerance）（以实际效果而言，分区相当于对通信的时限要求。系统如果不能在时限内达成数据一致性，就意味着发生了分区的情况，必须就当前操作在 C 和 A 之间做出选择。）

## 几个常用的 CAP 框架对比
|框架	|所属
|---|---|
|Eureka	|AP
|Zookeeper	|CP
|Consul	|CP

## 参考资料
   - [Java 工程师进阶知识完全扫盲](https://doocs.github.io/advanced-java/#/docs/distributed-system/distributed-system-cap)
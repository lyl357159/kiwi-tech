# TODO

## 面试阅读书籍
  - Java程序员求职突击面试手册.docx
  - [Java 工程师进阶知识完全扫盲](https://doocs.github.io/advanced-java/#/)


## 面试高阶问题
  - Linux IO模式及select、poll、epoll详解
  - 设计一个 Spring 框架你会怎么做？
  - 如果让你来设计一个 Dubbo 框架你会怎么做？
  - 如果让你来设计一个 MyBatis 框架你会怎么做？


## 大TODO
   - 职业规划

## 小TODO
  - ~~简历优化，更多描述知识图谱平台~~
  - mybabtis原理
  - ~~一致性hash算法~~
  - 集群元数据的维护有两种方式：集中式、**Gossip 协议**。Redis cluster 节点间采用 gossip 协议进行通信。
  -  ~~Hbase的批量数据操作~~
  -  ~~Redis全套学习笔记中的哨兵和集群~~
  -  ~~[ehcache](https://blog.csdn.net/weixin_42578444/article/details/80878611)~~
  -  ~~LRU、LFU~~
  -  http\hessian\thrift\gRPC\
  -  Dubbo最新版本
  -  常用数据结构和算法
  -  代理 javassist 
  -  spi bilibi
  -  时间轮
  -  一致性Hash
  -  dubbo 注册中心支持哪些
  -  etcd
  -  字典树（trie tree）
  -   maxHeap = new PriorityQueue<>(Comparator.reverseOrder());
        minHeap = new PriorityQueue<>(Integer::compareTo);
  - 归并排序
  - StatsD 监控 DataDog 链路：Spring Cloud Sleuth
  - spring 事务的传播机制
  - Redis的事务：https://blog.csdn.net/jiayi_yao/article/details/124689937
  - [面试 30 家公司，终于拿到 Offer](https://mp.weixin.qq.com/s/KKI6UNsBT8z0bI0hnJdDrQ)
  - GraalVM
  - [我画了 19 张图，帮你彻底搞懂 Redis](https://cloud.tencent.com/developer/article/1815975)
  - ~~破解百度下载~~
  - [常见的系统面试题](https://blog.51cto.com/csnd/5702114)
  - Http响应码 302重定向 短地址
  - Cassandra 
  - MongoDB
  - 云原生
  - Sidecar，lstio [Sidecar 架构模式](https://zhuanlan.zhihu.com/p/445804080) 负载均衡、服务发现、认证授权、监控追踪、流量控制等分布式系统所需要的功能
  - Istio 为代表的第二代Service Mesh
  - Service Mesh 
    - [什么是 Service Mesh](https://zhuanlan.zhihu.com/p/61901608)
    - [初识 Service Mesh](https://baijiahao.baidu.com/s?id=1709259327958538327&wfr=spider&for=pc)
  - 使用Vue + Element 模拟一个指定的页面，并加入到测试项目导航中

## 一些新的知识
  - RAP Mock数据 
  - ApiPost  Apipost = Postman + Swagger + Mock + Jmeter
  - Akka Akka是JAVA虚拟机JVM平台上构建高并发、分布式和容错应用的工具包和运行时。Akka用Scala语言写成，同时提供了Scala和JAVA的开发接口。
  - Riak Riak 是以 Erlang 编写的一个高度可扩展的分布式数据存储
  - Flutter [《Flutter实战·第二版》](https://book.flutterchina.club/)
  - Skia 2D绘图引擎
  - AOT （Ahead of time）即 “提前编译”.JIT（Just-in-time）即“即时编译”.


## 待学习完善方向
  - GO
  - 低代码开发平台
  - MongoDB
  - 成为社区开源贡献者


## 备忘
  - MAC应用下载: https://www.macbl.com/
  - 油小猴：https://www.youxiaohou.com/
  - ndm下载：http://neatdownloadmanager.com/index.php/en/
  

  int pre = 0; int ans = nums[0];
         for (int i = 0; i < nums.length; i++) {
              pre = Math.max(nums[i], pre + nums[i]);
              ans = Math.max(pre, ans);
         }
         return ans;


         强大的扩展程序BC是直角三角形但不
# Spark

## Spark的核心组件 
  - **Driver**
    - Spark 驱动器节点，用于执行 Spark 任务中的 main 方法，负责实际代码的执行工作。
    - Driver 在 Spark 作业执行时主要负责：
      - 1）将用户查询转化为任务；
      - 2）在 Executor 之间调度任务；
      - 3）跟踪 Executor 的执行情况；
      - 4）通过 UI 展示查询运行情况；
  - **Executor**
    - Spark Executor 节点是一个 JVM 进程，负责在 Spark 作业中运行具体任务，任务彼此之间相互独立。Spark 应用启动时，Executor 节点被同时启动，并且始终伴随着整个 Spark 应用的生命周期而存在。如果有 Executor 节点发生了故障或崩溃，Spark 应用也可以继续执行，会将出错节点上的任务调度到其他Executor 节点上继续运行。
    - Executor 有两个核心功能：
      - 1）负责运行组成 Spark 应用的任务，并将结果返回给驱动器进程；
      - 2）它们通过自身的块管理器（Block Manager）为用户程序中要求缓存的 RDD
    - 提供内存式存储。RDD 是直接缓存在 Executor 进程内的，因此任务可以在运行时充分利用缓存数据加速运算。

---

## Apache Spark 的不同运行模式
  - **Local Mode**：在本地运行 Spark，数据存储在本地磁盘中，所有的 Spark 组件都运行在同一个进程中。在这种模式下，Spark 通常被用于开发、测试和调试。
  - **Standalone Mode**：在单个机器或多个机器组成的集群上运行 Spark。这种模式下，Spark 管理所有资源，包括 CPU、内存和网络，用于执行 Spark 应用程序。在这种模式下，Spark 通常被用于小型到中型规模的数据处理。
  - **Mesos**——国内用的少
  - **yarn集群模式**——Spark客户端直接连接Yarn,不需要额外构建Spark集群。国内生产上用的多。

## 根据Driver运行在哪分为：
  - Client模式和Cluster模式
  - 用户在提交任务给 Spark 处理时，以下两个参数共同决定了 Spark 的运行方式。
    –master MASTER_URL ：决定了 Spark 任务提交给哪种集群处理。
    –deploy-mode DEPLOY_MODE：决定了 Driver 的运行方式，可选值为 Client或者 Cluster。


---
## 参考资料
   - [spark四种运行模式](https://www.jianshu.com/p/3d47d58dd48e)
   - [Spark基础【五种运行模式】](https://blog.csdn.net/weixin_43923463/article/details/126183803)
---

- [返回首页](../../README.md)

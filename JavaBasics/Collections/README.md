# Java集合框架详解

## 📚 内容导航

- [集合框架概述](./Overview.md) - 集合框架的整体架构和设计理念
- [List详解](./List.md) - ArrayList、LinkedList等实现原理及性能分析
- [Map详解](./Map.md) - HashMap、TreeMap、LinkedHashMap等实现原理
- [Set详解](./Set.md) - HashSet、TreeSet等实现原理及应用场景
- [Queue详解](./Queue.md) - 队列家族及其并发实现
- [并发集合](./ConcurrentCollections.md) - 线程安全集合类详解
- [集合最佳实践](./BestPractices.md) - 集合使用的常见陷阱和最佳实践

## 🔍 核心知识图谱

```mermaid
graph TD
    A[Java集合框架] --> B[接口层]
    A --> C[实现层]
    A --> D[算法层]
    
    B --> B1[Collection]
    B --> B2[Map]
    B --> B3[Iterator]
    
    B1 --> B11[List]
    B1 --> B12[Set]
    B1 --> B13[Queue]
    
    C --> C1[通用实现]
    C --> C2[专用实现]
    C --> C3[并发实现]
    C --> C4[包装实现]
    
    D --> D1[排序算法]
    D --> D2[查找算法]
    D --> D3[遍历算法]
    
    C1 --> C11[ArrayList/LinkedList]
    C1 --> C12[HashSet/TreeSet]
    C1 --> C13[HashMap/TreeMap]
    
    C3 --> C31[ConcurrentHashMap]
    C3 --> C32[CopyOnWriteArrayList]
    C3 --> C33[BlockingQueue实现]
    
    classDef interface fill:#f9f,stroke:#333,stroke-width:2px;
    classDef implementation fill:#bbf,stroke:#333,stroke-width:2px;
    classDef algorithm fill:#bfb,stroke:#333,stroke-width:2px;
    
    class B,B1,B2,B3,B11,B12,B13 interface;
    class C1,C2,C3,C4,C11,C12,C13,C31,C32,C33 implementation;
    class D,D1,D2,D3 algorithm;
```

## 📊 集合性能对比

| 集合类型 | 随机访问 | 插入/删除头部 | 插入/删除中间 | 插入/删除尾部 | 内存占用 | 线程安全 |
|---------|---------|--------------|--------------|--------------|---------|---------|
| ArrayList | O(1) | O(n) | O(n) | O(1)* | 低 | 否 |
| LinkedList | O(n) | O(1) | O(1)** | O(1) | 高 | 否 |
| HashMap | O(1) | - | - | - | 中 | 否 |
| TreeMap | O(log n) | - | - | - | 中 | 否 |
| HashSet | O(1) | - | - | - | 中 | 否 |
| ConcurrentHashMap | O(1) | - | - | - | 中 | 是 |
| CopyOnWriteArrayList | O(1) | O(n) | O(n) | O(n) | 很高 | 是 |

\* 当需要扩容时为O(n)  
\** 需要先遍历到指定位置，总体为O(n)

## 🚀 学习路径

1. **基础阶段**：集合框架概述 → List → Map → Set
2. **进阶阶段**：Queue → Iterator深入 → Comparable/Comparator
3. **高级阶段**：并发集合 → 自定义集合实现 → 源码分析
4. **实战阶段**：性能优化 → 集合最佳实践 → 设计模式与集合

## 📖 扩展阅读

- [Collections工具类完全指南](./CollectionsUtil.md)
- [自定义集合实现](./CustomCollections.md)
- [JDK版本演进中的集合变化](./CollectionsEvolution.md)
- [集合框架面试题集锦](./InterviewQuestions.md)

## 📘 相关资源

- [返回Java基础首页](../README.md)
- [Java内存模型与并发编程](../JMM/README.md)
- [Java I/O体系](../IO/README.md)

---

© Java知识库 2023 
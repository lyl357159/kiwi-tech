# ZGC垃圾回收器详解

## ZGC简介

ZGC (Z Garbage Collector) 是JDK 11引入的一款低延迟垃圾回收器，设计目标是：
- 停顿时间不超过10ms
- 停顿时间不会随着堆大小或存活对象数量的增加而增加
- 支持TB级别的超大堆内存

ZGC特别适合对延迟敏感的应用，如实时交易系统、在线游戏、金融风控等需要保证系统持续快速响应的场景。在JDK 11中作为实验特性引入，并在JDK 15中被正式发布为产品特性。

## ZGC核心技术

### 1. 着色指针 (Colored Pointers)

ZGC的一项关键创新是使用了64位指针的高位标记位(metadata bits)来标记对象状态，这种技术称为**着色指针**。

在64位系统上，实际只需要42位或44位来表示内存地址(支持4TB或16TB内存)，剩余的高位可以用作额外信息。ZGC利用这些高位作为标记位：

- **Marked0 (M0)**：垃圾回收周期中的标记位
- **Marked1 (M1)**：垃圾回收周期中的标记位
- **Remapped (R)**：表示对象是否已被重定位
- **Finalizable**：表示对象是否可以被终结

通过着色指针，ZGC可以实现以下功能：
- 无需使用额外的GC专用数据结构
- 支持并发处理
- 快速判断对象状态

### 2. 读屏障 (Load Barrier)

ZGC使用了读屏障技术，在应用线程读取对象引用时，ZGC会检查指针的标记位，根据需要执行修正操作：

- 如果指针指向的对象已被重定位，则返回新位置
- 如果对象需要被标记，则在返回引用前进行标记

读屏障使ZGC能够在对象被移动的同时，保证应用程序始终能够访问到对象的正确位置，从而支持并发的对象重定位。

```java
Object load_reference(Object* reference) {
    Object obj = *reference;  // 加载引用
    if (needs_marking(obj)) {
        mark_object(obj);     // 标记对象
    }
    if (has_been_relocated(obj)) {
        obj = forwarding_table.get_new_address(obj); // 获取新地址
        *reference = obj;     // 更新引用
    }
    return obj;
}
```

### 3. 并发标记和转移

ZGC通过以下机制实现并发处理：

- **完全并发的标记**：在应用程序运行的同时识别存活对象
- **并发转移**：在应用程序运行的同时重定位对象
- **增量GC**：将大的GC操作拆分成多个小的GC操作

这种设计使ZGC能够保持极低的暂停时间，同时高效地回收内存。

### 4. 基于区域的内存管理

ZGC将堆内存分为大小不同的区域(Region)，主要有：

- **小型区域(Small Region)**：默认2MB
- **中型区域(Medium Region)**：默认32MB
- **大型区域(Large Region)**：大小不固定，用于存储超大对象

通过基于区域的管理，ZGC可以：
- 更灵活地进行内存分配
- 支持更高效的内存回收
- 减少内存碎片

## ZGC工作流程

ZGC的垃圾回收过程分为以下几个主要阶段：

### 1. 初始标记 (Initial Mark) - STW阶段

- 标记所有GC Roots直接引用的对象
- 此阶段需要STW (Stop-The-World)，但时间极短
- 使用读屏障确保后续引用访问时对象能够被正确标记

### 2. 并发标记 (Concurrent Mark)

- 从GC Roots开始遍历整个对象图，标记所有可达对象
- 完全并发，应用线程同时运行
- 通过读屏障处理并发修改问题

### 3. 重新标记 (Remark) - STW阶段

- 处理标记阶段结束后残留的少量未处理对象
- 需要STW，但时间很短

### 4. 并发准备转移 (Concurrent Prepare for Relocate)

- 选择需要被重新整理的区域（基于垃圾密度等指标）
- 创建转发表(Forwarding Table)，记录对象的新位置
- 完全并发，无需STW

### 5. 初始转移 (Initial Relocate) - STW阶段

- 重定位GC Roots直接引用的对象
- 需要STW，但时间极短

### 6. 并发转移 (Concurrent Relocate)

- 将存活对象复制到新的内存位置
- 完全并发，应用线程同时运行
- 通过读屏障确保应用程序总是能访问到对象的正确位置

### 7. 清理 (Cleanup)

- 释放空闲的区域
- 重置GC状态，为下一轮GC做准备

## ZGC与其他GC的对比

| 特性 | ZGC | G1 | CMS | Parallel GC |
|------|-----|----|----|------------|
| 首次引入版本 | JDK 11 | JDK 7 | JDK 1.4.2 | JDK 1.4 |
| 默认启用版本 | JDK 15 | JDK 9 | 非默认 | JDK 8 |
| 最大停顿时间 | <10ms | 数十ms到数百ms | 数百ms | 秒级 |
| 吞吐量 | 良好 | 良好 | 中等 | 优秀 |
| 内存开销 | 中等偏高 | 高 | 低 | 低 |
| 并发能力 | 标记+转移并发 | 标记并发，转移STW | 标记并发，转移STW | 全部STW |
| 碎片化处理 | 优秀 | 良好 | 差 | 中等 |
| 大堆表现 | 优秀 | 良好 | 差 | 中等 |
| CPU使用率 | 高 | 中高 | 中 | 低 |
| GC预测性 | 高 | 中 | 低 | 中 |
| 使用场景 | 低延迟、大堆 | 通用场景 | 已被ZGC/G1替代 | 批处理、吞吐优先 |

## ZGC调优参数

### 基本参数

```
-XX:+UseZGC                  # 启用ZGC
-Xms<size>                   # 最小堆大小
-Xmx<size>                   # 最大堆大小
-XX:+UnlockExperimentalVMOptions # JDK 15之前需要
```

### 内存管理参数

```
-XX:ZAllocationSpikeTolerance=2.0   # 分配速率容忍度
-XX:ZCollectionInterval=0           # GC触发间隔，0表示不主动触发
-XX:ZFragmentationLimit=25          # 最大碎片比例，默认25%
-XX:ZMarkStackSpaceLimit=8589934592 # 标记栈空间限制
```

### 并发控制参数

```
-XX:ConcGCThreads=N               # 并发GC线程数
-XX:ParallelGCThreads=N           # 停顿阶段GC线程数
-XX:ZProactive=true               # 是否启用主动模式
-XX:ZUncommit=true                # 是否允许内存返还给操作系统
-XX:ZUncommitDelay=300            # 内存返还延迟(秒)
```

### 日志和监控参数

```
-Xlog:gc                       # 基本GC日志
-Xlog:gc*                      # 详细GC日志
-Xlog:gc+phases=debug          # GC阶段详细日志
-Xlog:gc+heap=trace            # 堆管理详细日志
```

## ZGC使用最佳实践

### 1. 适用场景

ZGC最适合以下场景：
- 对延迟敏感的在线交易系统
- 实时数据处理系统
- 响应式微服务
- 交互式应用程序
- 需要处理大内存数据集的应用

### 2. 堆内存配置

- 建议将最小堆内存(-Xms)与最大堆内存(-Xmx)设置为相同值，避免堆大小调整导致的暂停
- 给ZGC提供足够的内存，通常比其他GC需要更多内存才能发挥最佳性能

### 3. CPU资源配置

- ZGC是CPU密集型的GC，需要分配足够的CPU资源
- 建议设置`-XX:ConcGCThreads`为CPU核心数的1/4到1/2
- 多核心CPU环境下ZGC表现更佳

### 4. 监控和调优

- 使用详细的GC日志分析ZGC行为：`-Xlog:gc*:file=gc.log:time:filecount=5,filesize=100m`
- 监控关键指标：
  - GC暂停时间
  - 标记和重定位时间
  - 内存使用情况
  - GC频率
- 针对问题优化：
  - 如果暂停时间过长，检查GC Roots数量
  - 如果标记时间过长，考虑增加并发线程数
  - 如果重定位时间过长，检查对象分配模式

### 5. JDK版本选择

- JDK 11-14：ZGC作为实验特性，需要`-XX:+UnlockExperimentalVMOptions`
- JDK 15+：ZGC作为正式特性，无需解锁
- JDK 16+：进一步优化，增加对类卸载等功能的支持
- JDK 17 LTS：长期支持版本，推荐生产环境使用
- JDK 21 LTS：最新长期支持版本，ZGC性能更优

## ZGC日志分析

### 示例日志解析

```
[2.606s][info][gc,start    ] GC(0) Garbage Collection (Warmup)
[2.606s][info][gc,phases   ] GC(0) Pause Mark Start 0.015ms
[2.628s][info][gc,phases   ] GC(0) Concurrent Mark 21.692ms
[2.628s][info][gc,phases   ] GC(0) Pause Mark End 0.018ms
[2.628s][info][gc,phases   ] GC(0) Concurrent Process Non-Strong References 0.173ms
[2.629s][info][gc,phases   ] GC(0) Concurrent Reset Relocation Set 0.099ms
[2.630s][info][gc,phases   ] GC(0) Concurrent Select Relocation Set 1.019ms
[2.630s][info][gc,phases   ] GC(0) Pause Relocate Start 0.015ms
[2.638s][info][gc,phases   ] GC(0) Concurrent Relocate 8.125ms
[2.638s][info][gc,load     ] GC(0) Load: 5.73/5.80/5.57
[2.638s][info][gc,mmu      ] GC(0) MMU: 2ms/94.9%, 5ms/97.9%, 10ms/98.8%, 20ms/99.4%, 50ms/99.7%, 100ms/99.9%
[2.638s][info][gc,marking  ] GC(0) Mark: 2 stripe(s), 2 proactive flush(es), 1 terminate flush(es), 0 completion(s), 0 continuation(s)
[2.638s][info][gc,reloc    ] GC(0) Relocation: Successful, 8M relocated
[2.638s][info][gc,nmethod  ] GC(0) NMethods: 441 registered, 0 unregistered
[2.638s][info][gc,metaspace] GC(0) Metaspace: 12M used, 12M committed, 12M reserved
[2.638s][info][gc,ref      ] GC(0) Soft: 396 encountered, 0 discovered, 0 enqueued
[2.638s][info][gc,ref      ] GC(0) Weak: 499 encountered, 354 discovered, 0 enqueued
[2.638s][info][gc,ref      ] GC(0) Final: 485 encountered, 0 discovered, 0 enqueued
[2.638s][info][gc,ref      ] GC(0) Phantom: 16 encountered, 9 discovered, 0 enqueued
[2.638s][info][gc,heap     ] GC(0) Min Capacity: 8M(0%)
[2.638s][info][gc,heap     ] GC(0) Max Capacity: 4096M(100%)
[2.638s][info][gc,heap     ] GC(0) Soft Max Capacity: 4096M(100%)
[2.638s][info][gc,heap     ] GC(0) Used: 29M(1%)
[2.638s][info][gc,heap     ] GC(0) Live: 21M(1%)
[2.638s][info][gc,heap     ] GC(0) Allocated: 21M(1%)
[2.638s][info][gc,heap     ] GC(0) Garbage: 8M(0%)
[2.638s][info][gc,heap     ] GC(0) Reclaimed: 0M(0%)
[2.638s][info][gc,tlab     ] GC(0) TLABs: 4 created, 3 torn down, 1 reutilized, 113 waste (avg 37781B), 4341376B slow allocs
[2.638s][info][gc,stats    ] GC(0) Duration: 32.5ms
```

### 关键指标解读

1. **暂停时间**：
   - `Pause Mark Start`, `Pause Mark End`, `Pause Relocate Start` 三个阶段为STW阶段
   - 在上面的示例中，总STW时间为0.015+0.018+0.015=0.048ms，远低于10ms目标

2. **并发阶段时间**：
   - `Concurrent Mark`：并发标记阶段时间
   - `Concurrent Relocate`：并发重定位阶段时间
   - 这些阶段不会暂停应用程序

3. **内存使用情况**：
   - `Used`: 当前已使用内存
   - `Live`: 存活对象占用内存
   - `Allocated`: GC周期内分配的内存
   - `Garbage`: 可回收垃圾内存
   - `Reclaimed`: 已回收内存

4. **MMU (Minimum Mutator Utilization)**：
   - 表示在不同时间窗口内应用线程最小可用时间百分比
   - 例如 `10ms/98.8%` 表示在任何10ms窗口内，应用程序至少有98.8%的时间在运行

## ZGC内部机制与源码分析

### 对象分配路径

ZGC中的对象分配主要有三种路径：

1. **TLAB分配**：线程本地分配缓冲区，用于小对象分配
2. **慢速分配**：当TLAB不足时的备选路径
3. **大对象分配**：直接在堆上分配大对象

核心源码逻辑(C++伪代码)：
```cpp
HeapWord* ZGC::mem_allocate(size_t size, bool large_object) {
  if (large_object) {
    // 大对象直接在堆上分配
    return allocate_large_object(size);
  }
  
  // 尝试在TLAB中分配
  HeapWord* result = thread->tlab().allocate(size);
  if (result != NULL) {
    return result;
  }
  
  // TLAB分配失败，尝试慢速分配
  return allocate_slow(size);
}
```

### 并发标记实现

ZGC并发标记的核心是基于三色标记算法的变种，结合了读屏障技术：

```cpp
void ZGC::mark_object(oop obj) {
  // 如果对象已标记，直接返回
  if (is_marked(obj)) {
    return;
  }
  
  // 标记对象
  set_mark_bit(obj);
  
  // 将对象添加到标记栈，用于后续处理
  mark_stack.push(obj);
}

void ZGC::process_mark_stack() {
  while (!mark_stack.is_empty()) {
    oop obj = mark_stack.pop();
    
    // 处理对象的所有引用字段
    for (每个引用字段 field) {
      oop referenced = load_reference(field);
      if (referenced != NULL) {
        mark_object(referenced);
      }
    }
  }
}
```

## JDK 17/21 中的ZGC优化

### JDK 16-17 主要改进

1. **并发线程栈处理**：
   - 在JDK 15之前，线程栈扫描是在STW阶段完成的
   - JDK 16之后，将大部分线程栈扫描工作移至并发阶段，进一步减少STW时间

2. **支持类卸载**：
   - JDK 16添加了对类卸载的支持
   - 可以回收不再使用的类，减少元空间压力

3. **避免过早触发GC**：
   - 改进了GC触发机制，避免不必要的GC周期
   - 更好地处理内存分配尖峰

### JDK 18-21 主要改进

1. **分层编译支持优化**：
   - 改进了对分层编译的支持，降低了ZGC对JIT编译的影响

2. **GC并行化优化**：
   - 将更多GC操作并行化处理，提高GC效率

3. **动态线程数调整**：
   - 根据系统负载动态调整GC线程数，平衡GC效率和系统资源

4. **预留堆内存优化**：
   - 改进了内存分配和释放策略，更高效地使用物理内存

## 实战案例：ZGC在高并发系统中的应用

### 案例背景

某金融交易系统，每秒处理数万笔交易请求，要求99.99%的请求响应时间不超过50ms。系统面临的挑战：
- 大量小对象快速分配和释放
- 部分长寿命对象需长期保留
- 经常出现GC导致的系统响应时间抖动

### 调优过程

1. **基线测试**：
   - 使用G1 GC，99.9%的请求响应时间在40ms以内
   - 但99.99%的请求响应时间高达120ms
   - GC日志显示偶发性长暂停(100-200ms)

2. **切换到ZGC**：
   - 使用基本ZGC配置：`-XX:+UseZGC -Xms16g -Xmx16g`
   - 99.9%的请求响应时间降至35ms
   - 99.99%的请求响应时间降至60ms，但仍不满足要求

3. **ZGC优化**：
   ```
   -XX:+UseZGC 
   -Xms20g -Xmx20g
   -XX:ConcGCThreads=4
   -XX:ZCollectionInterval=120
   -XX:+UnlockDiagnosticVMOptions
   -XX:+ZStatistics
   ```

4. **结果分析**：
   - 99.99%的请求响应时间降至45ms，满足要求
   - GC暂停时间稳定在5ms以内
   - CPU使用率增加约15%，但在可接受范围内

### 监控与故障排查

设置全面的监控系统，关注以下指标：
- JVM暂停时间
- 对象分配率
- 内存使用情况
- GC频率和持续时间
- 应用程序响应时间

### 关键经验

1. 增加堆内存，给ZGC足够的工作空间
2. 合理配置并发GC线程数
3. 减少不必要的对象分配
4. 使用详细GC日志进行持续监控和优化
5. 相比G1，ZGC需要更多的CPU资源，但能提供更稳定的延迟表现
6. 生产环境建议使用JDK 17或21等LTS版本

## 总结

ZGC是一款面向低延迟场景的高性能垃圾回收器，其核心优势包括：

1. **极低的暂停时间**：无论堆大小如何，停顿时间通常都不超过10ms
2. **可扩展性强**：适用于从小内存到TB级内存的各种场景
3. **稳定的性能**：GC行为的可预测性高
4. **持续改进**：从JDK 11到JDK 21，不断获得性能和功能增强

如果你的应用对延迟敏感、需要大内存、有稳定响应时间要求，ZGC是非常值得考虑的选择。随着JDK的发展，ZGC的性能还会持续提升，成为Java生态系统中越来越重要的一部分。

---

- [返回GC目录](../GC/)
- [返回JVM目录](../JVM.md)
- [返回首页](../../../README.md) 
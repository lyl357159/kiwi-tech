# JVM (Java Virtual Machine)

## JVM的组成
- **类装载器(Class Loader)**: 负责加载字节码到内存，并转换为JVM可识别的结构
- **执行引擎(Execution Engine)**: 负责执行字节码指令
- **本地库接口(Native Interface)**: JNI，提供与本地方法库交互的接口
- **运行时数据区(Runtime Data Area)**: JVM内存模型，用于存储运行时数据
   
## 运行时数据区（JVM内存模型）
- **堆(Heap)**：
  - **新生代(Young Generation)** [占堆内存的1/3]
    - **伊甸区(Eden Space)**: 新对象分配区域 [占新生代的8/10]
    - **幸存区S0(Survivor 0)**: Minor GC后存活对象 [占新生代的1/10]
    - **幸存区S1(Survivor 1)**: Minor GC后存活对象 [占新生代的1/10]
  - **老年代(Old Generation)** [占堆内存的2/3]
    - 存放长期存活的对象或大对象
    - 通常由Major GC或Full GC进行回收
  - 线程共享，GC主要管理区域

- **元空间(Metaspace)**：
  - JDK 1.8后代替永久代(PermGen)，使用本地内存
  - 存储类的元数据信息：类结构、方法数据、方法代码等
  - 存储运行时常量池(Runtime Constant Pool)
  - 按需分配，可动态调整大小，理论上只受系统内存限制

- **Java虚拟机栈(JVM Stack)**：
  - 线程私有，生命周期与线程相同
  - 每个方法执行时创建一个栈帧(Stack Frame)
  - 栈帧包含：局部变量表、操作数栈、动态链接、方法返回地址等
  - 可能抛出两种异常：StackOverflowError和OutOfMemoryError

- **本地方法栈(Native Method Stack)**：
  - 线程私有，为本地方法(Native Method)服务
  - 执行Java调用的C/C++等语言的代码

- **程序计数器(Program Counter Register)**：
  - 线程私有，大小很小
  - 记录当前线程执行字节码的行号指示器
  - 唯一不会产生OutOfMemoryError的内存区域

## 类加载机制
### 类加载过程
1. **加载(Loading)**：
   - 通过类名获取定义此类的二进制字节流
   - 将字节流所代表的静态存储结构转化为方法区运行时数据结构
   - 在内存中生成代表这个类的java.lang.Class对象

2. **链接(Linking)**：
   - **验证(Verification)**：确保字节码安全可执行
   - **准备(Preparation)**：为类变量分配内存并设置初始值
   - **解析(Resolution)**：将符号引用替换为直接引用

3. **初始化(Initialization)**：
   - 执行类构造器<clinit>()方法
   - 为类变量赋予正确的初始值
   
### 类加载器层次
- **启动类加载器(Bootstrap ClassLoader)**：
  - C++实现，加载JAVA_HOME/lib下的类库
  
- **扩展类加载器(Extension ClassLoader)**：
  - JDK 9后改名为PlatformClassLoader
  - 加载JAVA_HOME/lib/ext目录中的类库
  
- **应用类加载器(Application ClassLoader)**：
  - 加载用户classpath下的类库
  
- **自定义类加载器**：
  - 通过继承ClassLoader实现
  - 打破双亲委派模型的常见方式

### 双亲委派模型
- 加载过程：
  1. 类加载请求到达时，先交给父加载器处理
  2. 如果父加载器无法完成，子加载器才尝试
  - 优点：避免类重复加载，保证Java核心API不被篡改

## JVM常用参数
- **堆内存相关**：
  - `-Xms<size>`: 初始堆大小，如-Xms2g
  - `-Xmx<size>`: 最大堆大小，如-Xmx2g
  - `-Xmn<size>`: 新生代大小，如-Xmn512m
  - `-XX:SurvivorRatio=n`: Eden区与Survivor区比例，默认8:1:1
  - `-XX:NewRatio=n`: 新生代与老年代比例，默认1:2

- **元空间相关**：
  - `-XX:MetaspaceSize=<size>`: 元空间初始大小
  - `-XX:MaxMetaspaceSize=<size>`: 元空间最大大小

- **垃圾回收器选择**：
  - `-XX:+UseSerialGC`: 串行回收器
  - `-XX:+UseParallelGC`: 并行回收器
  - `-XX:+UseConcMarkSweepGC`: CMS回收器
  - `-XX:+UseG1GC`: G1回收器
  - `-XX:+UseZGC`: Z垃圾回收器(JDK11引入，JDK15默认可用)
  - `-XX:+UseShenandoahGC`: Shenandoah回收器

- **GC日志相关**：
  - `-XX:+PrintGCDetails`: 打印GC详细信息
  - `-XX:+PrintGCDateStamps`: 打印GC时间戳
  - `-Xloggc:<file>`: GC日志输出到文件

- **故障排查**：
  - `-XX:+HeapDumpOnOutOfMemoryError`: OOM时自动生成堆转储文件
  - `-XX:HeapDumpPath=<path>`: 指定堆转储文件路径
  - `-XX:OnOutOfMemoryError="<cmd>"`: OOM时执行指定命令

## JDK工具集
- **监控和故障排查**：
  - `jps`: 显示当前所有Java进程PID
  - `jstat`: 监视JVM统计信息
  - `jinfo`: 查看和修改JVM参数
  - `jmap`: 生成堆转储快照
  - `jhat`: 分析堆转储快照
  - `jstack`: 生成线程快照
  - `jcmd`: 执行JVM诊断命令

- **可视化工具**：
  - `JConsole`: Java监视和管理控制台
  - `VisualVM`: 多合一故障排查工具
  - `JMC (Java Mission Control)`: 包含JFR的强大分析工具
  - `Flight Recorder (JFR)`: 低开销诊断事件收集框架

## JVM调优实战
### 常见问题定位
1. **内存泄漏**：
   - 使用`jmap -dump:format=b,file=<file> <pid>`生成堆转储
   - 使用MAT或VisualVM分析堆转储文件

2. **CPU过高**：
   - 使用`top -Hp <pid>`找出高CPU线程
   - 使用`jstack <pid>`生成线程栈
   - 将线程ID转换为16进制，在线程栈中查找对应信息

3. **频繁GC**：
   - 使用`jstat -gcutil <pid> <interval> <count>`观察GC情况
   - 分析GC日志定位问题

### 调优策略
1. **响应时间优先**：
   - 选择低延迟GC (CMS/G1/ZGC)
   - 增大新生代比例，降低Minor GC频率
   - 考虑使用大页内存`-XX:+UseLargePages`

2. **吞吐量优先**：
   - 选择Parallel GC
   - 适当增大堆内存
   - 调整GC触发比例`-XX:InitiatingHeapOccupancyPercent`

3. **内存受限场景**：
   - 减小堆内存，增加GC频率
   - 考虑Serial GC
   - 减少临时对象创建，强化对象复用

## Hotspot JVM架构
- **即时编译器(JIT Compiler)**：
  - C1编译器(客户端编译器)：快速编译，优化程度低
  - C2编译器(服务端编译器)：编译慢，优化程度高
  - 分层编译(Tiered Compilation)：结合C1和C2优点

- **内存分配与回收策略**：
  - TLAB(Thread Local Allocation Buffer)：线程私有分配缓冲区
  - 逃逸分析：减少堆内存分配，栈上分配
  - 标量替换：分解对象，直接创建若干个局部变量

- **指针压缩**：
  - 启用`-XX:+UseCompressedOops`
  - 32位引用表示64位内存地址
  - 堆内存小于32GB时有效

## JDK 17/21 新特性
- **ZGC增强**：
  - JDK 15开始默认可用，JDK 17进一步优化
  - 停顿时间不随堆增长而增加(10ms以内)

- **Shenandoah GC**：
  - 与ZGC类似的低延迟垃圾收集器
  - JDK 17中更加稳定成熟

- **紧凑字符串(Compact Strings)**：
  - JDK 9引入，默认启用
  - Latin-1字符使用单字节存储，节约内存

- **Java模块系统(JPMS)**：
  - JDK 9引入，企业应用逐渐采用
  - 模块化依赖提高安全性和性能

- **JFR事件流API**：
  - JDK 14引入，实时监控JVM运行状态
  - JDK 17/21持续优化

---

- [回到首页](../../README.md)
# Java类加载机制详解

## 类加载的全流程

Java类从被加载到JVM内存，到卸载出内存，整个生命周期包括：**加载(Loading)、验证(Verification)、准备(Preparation)、解析(Resolution)、初始化(Initialization)、使用(Using)和卸载(Unloading)**七个阶段，其中验证、准备、解析统称为"链接(Linking)"阶段。

![类加载生命周期](../../../Resources/images/classloader-lifecycle.png)

### 1. 加载阶段(Loading)

加载是类加载过程的第一个阶段。在这个阶段，JVM主要完成以下三件事：

1. **通过类的全限定名获取定义此类的二进制字节流**
   - 从ZIP包读取（常见的JAR、WAR等归档文件）
   - 从网络中获取（例如Applet）
   - 运行时计算生成（动态代理技术）
   - 由其他文件生成（JSP文件）
   - 从数据库中读取（某些中间件服务器可能将程序安装到数据库中）

2. **将字节流所代表的静态存储结构转化为方法区的运行时数据结构**
   - 方法区存储了类的信息、常量、静态变量等数据

3. **在内存中生成代表这个类的Class对象，作为方法区这个类的各种数据的访问入口**
   - 这个Class对象是对方法区中类数据的访问入口，也是反射的起点

### 2. 验证阶段(Verification)

验证是连接阶段的第一步，目的是确保Class文件中的字节流包含的信息符合JVM规范，不会危害JVM自身安全。验证阶段大致会完成4个阶段的检验动作：

1. **文件格式验证**：验证字节流是否符合Class文件格式规范
   - 魔数(0xCAFEBABE)检查
   - 版本号检查
   - 常量池中的常量类型是否支持
   - ...

2. **元数据验证**：对字节码描述的信息进行语义分析
   - 类是否有父类(Object除外)
   - 类是否继承了被final修饰的类
   - 非抽象类是否实现了其父类或接口中要求实现的所有方法
   - ...

3. **字节码验证**：通过数据流和控制流分析，确定程序语义是合法的、符合逻辑的
   - 保证跳转指令不会跳转到方法体以外的字节码指令上
   - 保证方法体中的类型转换是有效的
   - ...

4. **符号引用验证**：在解析阶段将符号引用转化为直接引用时进行
   - 符号引用中通过字符串描述的全限定名是否能找到对应的类
   - 指定类中是否存在符合方法的字段描述符及简单名称所描述的方法和字段
   - 符号引用中的类、字段、方法的访问性是否可被当前类访问
   - ...

### 3. 准备阶段(Preparation)

准备阶段是为**类变量(static修饰的成员变量)**分配内存并设置类变量初始值的阶段。这些变量所使用的内存都将在方法区中进行分配。

需要注意的是：
- 这时候进行内存分配的仅包括类变量（静态变量），不包括实例变量
- 这里所设置的初始值为默认值，而非代码中被显式赋予的值

例如，对于以下代码：
```java
public static int value = 123;
```
在准备阶段，JVM会为value分配内存，并将其初始值设置为0，而非123。赋值为123的动作将在初始化阶段执行。

但如果是final修饰的静态变量，则会在准备阶段直接赋值：
```java
public static final int value = 123;
```
在准备阶段，JVM会为value分配内存，并将其初始值设置为123。

### 4. 解析阶段(Resolution)

解析阶段是虚拟机将常量池内的符号引用替换为直接引用的过程。

- **符号引用(Symbolic References)**：以一组符号来描述所引用的目标，与虚拟机实现的内存布局无关，引用的目标并不一定已经加载到内存中。
- **直接引用(Direct References)**：直接指向目标的指针、相对偏移量或是一个能间接定位到目标的句柄，与虚拟机实现的内存布局相关。

解析动作主要针对类或接口、字段、类方法、接口方法、方法类型、方法句柄和调用点限定符7类符号引用进行。解析可能在初始化之前也可能在初始化之后，取决于JVM实现和当前执行代码的行为。

### 5. 初始化阶段(Initialization)

初始化是类加载过程的最后一步，真正执行Java程序代码（字节码）的阶段。初始化阶段就是执行类构造器`<clinit>()`方法的过程。

`<clinit>()`方法是由编译器自动收集类中的所有类变量的赋值动作和静态语句块(static{}块)中的语句合并产生的，编译器收集的顺序是由语句在源文件中出现的顺序所决定的。

注意事项：
- `<clinit>()`方法不需要显式调用父类的构造器，JVM会保证子类的`<clinit>()`方法执行前，父类的`<clinit>()`方法已经执行完毕。
- 如果一个类中没有静态语句块，也没有对类变量的赋值操作，那么编译器可以不为这个类生成`<clinit>()`方法。
- 接口中不能使用静态语句块，但仍然有类变量赋值操作，因此接口与类一样都会生成`<clinit>()`方法。
- JVM会保证一个类的`<clinit>()`方法在多线程环境中被正确地加锁、同步，如果多个线程同时去初始化一个类，那么只会有一个线程去执行这个类的`<clinit>()`方法，其他线程都需要阻塞等待。

## 类加载的时机

JVM规范严格规定了有且只有以下六种情况必须立即对类进行"初始化"（而加载、验证、准备自然是在此之前完成）：
1. 遇到`new`、`getstatic`、`putstatic`或`invokestatic`这四条字节码指令时：
   - 使用new关键字实例化对象
   - 读取或设置一个类的静态字段（被final修饰、已在编译期把结果放入常量池的静态字段除外）
   - 调用一个类的静态方法

2. 使用`java.lang.reflect`包的方法对类进行反射调用时。

3. 当初始化一个类时，如果发现其父类还没有进行过初始化，则需要先触发其父类的初始化。

4. 当JVM启动时，用户需要指定一个要执行的主类（包含main()方法的类），JVM会先初始化这个主类。

5. JDK 7新加入的动态语言支持：如果一个`java.lang.invoke.MethodHandle`实例最后的解析结果为`REF_getStatic`、`REF_putStatic`、`REF_invokeStatic`的方法句柄，且句柄对应的类没有初始化，则需要先触发该类的初始化。

6. 当接口中定义了JDK 8新加入的默认方法（default方法）时，如果该接口的实现类发生了初始化，那么该接口也会被初始化。

对于接口的初始化，与类不同的是，接口的初始化不会执行父接口的初始化。只有当接口中定义的静态变量被使用时，才会导致该接口初始化。

## 类加载器

类加载器是JVM提供的一种用于加载类的机制，通过一个类的全限定名来获取描述此类的二进制字节流。

### 类加载器的层次结构

在JDK 8及之前，Java中存在三种默认的类加载器：
1. **启动类加载器(Bootstrap ClassLoader)**：
   - 负责加载`<JAVA_HOME>/lib`目录中的，或通过-Xbootclasspath参数指定路径下的，且被虚拟机识别的（按文件名识别，如rt.jar、tools.jar）类库到虚拟机内存中。
   - 启动类加载器是由C++语言实现的，属于虚拟机自身的一部分。
   - 在Java代码中获取启动类加载器的方式是通过`ClassLoader.getSystemClassLoader().getParent().getParent()`，但这个方法通常会返回null，因为Bootstrap ClassLoader通常不被Java程序直接引用。

2. **扩展类加载器(Extension ClassLoader)**：
   - 负责加载`<JAVA_HOME>/lib/ext`目录中的，或通过java.ext.dirs系统变量指定路径下的类库。
   - 由sun.misc.Launcher$ExtClassLoader实现（JDK 8）。
   - JDK 9后，更名为平台类加载器(Platform ClassLoader)。

3. **应用程序类加载器(Application ClassLoader)**：
   - 负责加载用户类路径(ClassPath)上所指定的类库。
   - 由sun.misc.Launcher$AppClassLoader实现（JDK 8）。
   - 是Java应用程序中的默认类加载器，通过`ClassLoader.getSystemClassLoader()`可以获取到。

在JDK 9中，JDK被模块化，类加载器的结构也发生了变化：

1. **启动类加载器(Bootstrap ClassLoader)**：
   - 仍然由C++实现，但只加载少量核心模块。

2. **平台类加载器(Platform ClassLoader)**：
   - 由`jdk.internal.loader.ClassLoaders$PlatformClassLoader`实现。
   - 负责加载JDK平台中的核心API，取代了扩展类加载器。

3. **应用程序类加载器(Application ClassLoader)**：
   - 由`jdk.internal.loader.ClassLoaders$AppClassLoader`实现。
   - 负责加载应用程序ClassPath路径下的类库。

4. **自定义类加载器**：
   - 开发人员可以自定义类加载器来满足特殊需求。
   - 通过继承`java.lang.ClassLoader`类来实现。

![类加载器层次](../../../Resources/images/classloader-hierarchy.png)

### 双亲委派模型

双亲委派模型要求除了顶层的启动类加载器外，其他的类加载器都应该有自己的父类加载器。这里的父子关系不是通过继承实现的，而是通过组合关系来实现的。

双亲委派模型的工作流程如下：
1. 当一个类加载器收到类加载请求时，它首先不会自己去尝试加载这个类，而是把这个请求委派给父类加载器去完成。
2. 每一个层次的类加载器都是如此，因此所有的加载请求最终都应该传送到最顶层的启动类加载器中。
3. 只有当父类加载器反馈自己无法完成这个加载请求（它的搜索范围中没有找到所需的类）时，子类加载器才会尝试自己去完成加载。

**双亲委派模型的优点**：
- **避免类的重复加载**：当父类加载器已经加载了该类时，就没有必要子类加载器再加载一次。
- **保证Java核心API的安全性**：例如，如果用户自己编写了一个`java.lang.Object`类，由于双亲委派模型的存在，它永远不会被加载，因为Bootstrap ClassLoader会首先加载JDK自带的`java.lang.Object`类。

![双亲委派过程](../../../Resources/images/classloader-delegation.png)

### 自定义类加载器

自定义类加载器通常需要继承`java.lang.ClassLoader`类，重写`findClass()`方法，而不是重写`loadClass()`方法，因为`loadClass()`方法是实现双亲委派模型的关键方法。

如果要破坏双亲委派模型，可以重写`loadClass()`方法。

**自定义类加载器的应用场景**：
- 加密解密（对字节码文件加密，类加载器负责解密）
- 从非标准来源加载代码
- 以特殊方式加载类，如动态创建类
- 热部署/热加载
- 类隔离（不同的应用使用不同的类加载器）

**示例代码**：简单的自定义类加载器
```java
public class CustomClassLoader extends ClassLoader {
    
    private String classPath;
    
    public CustomClassLoader(String classPath) {
        this.classPath = classPath;
    }
    
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        try {
            byte[] classData = loadClassData(name);
            if (classData == null) {
                throw new ClassNotFoundException();
            }
            return defineClass(name, classData, 0, classData.length);
        } catch (IOException e) {
            throw new ClassNotFoundException("Could not load class data: " + name, e);
        }
    }
    
    private byte[] loadClassData(String className) throws IOException {
        String fileName = classPath + File.separatorChar + 
                         className.replace('.', File.separatorChar) + ".class";
        try (InputStream ins = new FileInputStream(fileName);
             ByteArrayOutputStream baos = new ByteArrayOutputStream()) {
            
            int bufferSize = 4096;
            byte[] buffer = new byte[bufferSize];
            int bytesNumRead;
            while ((bytesNumRead = ins.read(buffer)) != -1) {
                baos.write(buffer, 0, bytesNumRead);
            }
            return baos.toByteArray();
        }
    }
}
```

### 打破双亲委派模型

在实际应用中，有时需要打破双亲委派模型，典型的例子包括：

1. **JNDI、JDBC等服务提供接口**：
   - 核心库提供接口，具体实现由厂商实现，但厂商实现的代码需要能被核心库加载调用。
   - JDK 1.2引入了线程上下文类加载器(Thread Context ClassLoader)来解决这个问题。

2. **OSGi模块化系统**：
   - OSGi实现了一套类加载机制，允许同一个类文件被不同的模块加载，实现类的隔离。
   - OSGi的类加载器实现了完全自定义的加载顺序。

3. **Tomcat等Web容器**：
   - 实现应用间的类隔离，同时又可共享部分库。
   - 使用复杂的类加载器层次结构。

4. **Java 9模块化系统**：
   - 引入了Layer概念，每个Layer都有自己的类加载器集合。
   - 类加载不仅要考虑双亲委派关系，还要考虑模块导出和依赖关系。

打破双亲委派模型的一般做法是重写`loadClass()`方法：
```java
@Override
public Class<?> loadClass(String name) throws ClassNotFoundException {
    // 如果是特定包下的类，优先自己加载
    if (name.startsWith("com.example.special")) {
        return findClass(name);
    }
    // 其他类遵循双亲委派
    return super.loadClass(name);
}
```

## 类加载器的隔离特性与命名空间

不同的类加载器可以加载同名的类，这些类在JVM中是不相等的。一个类由类本身和加载它的类加载器共同确定其在JVM中的唯一性。这就是类加载器的隔离特性。

JVM判断两个类是否相同，不仅要看类的全限定名是否相同，还要看加载这个类的类加载器是否相同。只有两个类来源于同一个Class文件，并且被同一个类加载器加载，这两个类才相等。

这种特性在某些场景下非常有用，例如：

1. **J2EE容器隔离**：每个应用有自己的类加载器，保证应用间的类隔离。
2. **热部署**：通过替换类加载器实现代码的热更新。
3. **OSGi框架**：每个Bundle有自己的类加载器，实现模块间的依赖和隔离。

## Java 9模块化系统下的类加载

Java 9引入了模块系统(JPMS)，对类加载机制产生了重大影响：

1. **模块描述符(module-info.java)**：定义模块的名称、依赖、导出包、服务等。
2. **强封装**：默认情况下，模块中的包对其他模块不可见，除非通过`exports`显式导出。
3. **模块路径**：类加载器不仅搜索类路径，还会搜索模块路径。
4. **Layer概念**：每个Layer包含一组模块和对应的类加载器。
5. **平台类加载器**：替代了扩展类加载器，加载JDK平台模块。

模块化系统下类加载的主要变化：
- 类加载时会检查模块的可读性(readability)
- 即使一个类是公开的(public)，如果其所在包没有被导出，其他模块也无法访问
- 反射操作需要考虑模块的开放性

## 类加载安全问题

类加载机制可能导致以下安全问题：

1. **篡改核心类库**：恶意代码尝试替换Java核心类库，双亲委派模型可以有效防止此类攻击。
2. **不安全的反序列化**：反序列化过程中可能加载恶意类，应使用白名单过滤。
3. **动态加载远程代码**：从网络加载类时应进行权限控制和代码来源验证。
4. **类路径注入**：恶意类可能被放置在应用的类路径中，应限制类路径访问权限。

## 性能考量

类加载对应用性能有显著影响，主要体现在以下方面：

1. **启动时间**：大量类的加载会延长应用启动时间。
   - 解决方案：延迟加载、AOT(Ahead-of-Time)编译。

2. **内存占用**：每个加载的类都占用方法区内存。
   - 解决方案：类卸载、限制类加载范围。

3. **类搜索开销**：过长的类路径会增加类搜索时间。
   - 解决方案：精简类路径、使用模块化系统。

4. **动态生成类的开销**：大量动态生成类（如通过反射、代理）会增加内存压力。
   - 解决方案：复用生成的类、限制生成类的数量。

## JDK 17/21 类加载的新特性

JDK 17和21引入了以下影响类加载的新特性：

1. **强封装JDK内部API**：限制反射访问JDK内部类。
2. **密封类(Sealed Classes)**：限制哪些类可以继承特定类。
3. **向量API(Vector API)**：提供特殊的向量操作类。
4. **外部函数和内存API**：提供访问外部本地函数的能力。
5. **禁用不安全的反射访问**：默认禁止通过反射访问私有成员。

## 最佳实践

1. **避免自定义ClassLoader的滥用**：
   - 只在确实需要时才使用自定义ClassLoader
   - 尽量不要破坏双亲委派模型

2. **控制类加载范围**：
   - 避免过大的类路径(ClassPath)
   - 使用模块化系统限制可访问的类

3. **注意类的卸载**：
   - 类的卸载只能在其ClassLoader被回收时发生
   - 使用弱引用管理动态生成的类

4. **类加载监控**：
   - 使用`-XX:+TraceClassLoading`、`-XX:+TraceClassUnloading`跟踪类加载/卸载
   - 使用JFR(Java Flight Recorder)监控类加载事件

5. **处理NoClassDefFoundError**：
   - 确保所有必要的类都在类路径中
   - 检查类依赖和加载顺序
   - 排查类加载器隔离问题

---

- [回到JVM目录](./JVM.md)
- [回到首页](../../README.md) 
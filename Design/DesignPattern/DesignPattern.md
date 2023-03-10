# 设计模式
## 六大原则
  - **开闭原则（Open Close Principle）**
    开闭原则的意思是：对扩展开放，对修改关闭。在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。简言之，是为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类，后面的具体设计中我们会提到这点。
    >
  - **里氏替换原则（Liskov Substitution Principle）**
    里氏代换原则是面向对象设计的基本原则之一。 里氏代换原则中说，任何基类可以出现的地方，子类一定可以出现。LSP 是继承复用的基石，只有当派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用，而派生类也能够在基类的基础上增加新的行为。里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤就是抽象化，而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。
    >
  - **依赖倒转原则（Dependence Inversion Principle）**
    这个原则是开闭原则的基础，具体内容：针对接口编程，依赖于抽象而不依赖于具体。
    >
  - **接口隔离原则（Interface Segregation Principle）**
    这个原则的意思是：使用多个隔离的接口，比使用单个接口要好。它还有另外一个意思是：降低类之间的耦合度。由此可见，其实设计模式就是从大型软件架构出发、便于升级和维护的软件设计思想，它强调降低依赖，降低耦合。
    >
  - **迪米特法则，又称最少知道原则（Demeter Principle）**
    最少知道原则是指：一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立。
    >
  - **合成复用原则（Composite Reuse Principle）**   
    合成复用原则是指：尽量使用合成/聚合的方式，而不是使用继承。


## 设计模式类型
  - 创建型模式
    - 单例模式
    - 工厂模式
    - 抽象工厂模式
    - 建造者模式(builder)
    - 原型模式(clone)
  - 结构型模式
    - 适配器模式(AdvancedMediaPlayer=VlcPlayer+Mp4Player)
    - 桥接模式(合并不同维度能力，比如画形状 + 画颜色)
    - 过滤器模式(Filter)
    - 组合模式(人员组织体系)
    - 装饰器模式(增加扩展能力)
    - 外观模式(即门面模式，医院接待员)
    - 享元模式(IntegerCache)
    - 代理模式(和适配器模式的区别：适配器模式主要改变所考虑对象的接口，而代理模式不能改变所代理类的接口。 和装饰器模式的区别：装饰器模式为了增强功能，而代理模式是为了加以控制。)
- 行为型模式
    - 责任链模式(有多个对象可以处理同一个请求,LoggerAppender)
    - 命令模式(认为是命令的地方都可以使用命令模式,新的命令可以很容易添加)
    - 解释器模式
    - 迭代器模式(访问一个聚合对象的内容而无须暴露它的内部表示)
    - 中介者模式(WTO, MVC中的C)
    - 备忘录模式(存档、回滚)
    - 观察者模式(通知、触发)
    - 状态模式(行为随状态改变而改变的场景)
    - 空对象模式
    - 策略模式
    - 模板模式(有多个子类共有的方法，且逻辑相同。)
    - 访问者模式(电脑配件)

---
## 设计模式举例
### 单例模式
   - [参考资料：菜鸟教程单例模式](https://www.runoob.com/design-pattern/singleton-pattern.html)
   - 双检锁/双重校验锁（DCL，即 double-checked locking），这种方式采用双锁机制，安全且在多线程情况下能保持高性能。
   ```java 
   public class Singleton {  
    private volatile static Singleton singleton;  
    private Singleton (){}  
    public static Singleton getSingleton() {  
    if (singleton == null) {  
        synchronized (Singleton.class) {  
            if (singleton == null) {  
                singleton = new Singleton();  
            }  
        }  
    }  
    return singleton;  
    }  
}
   ```

## 参考资料
  - [菜鸟：设计模式](https://www.runoob.com/design-pattern/design-pattern-intro.html)
  

  - 待完善知识点
    - hive
    - hbase
    - mysql 执行过程 log
    - sentinel
# Questions

## Spring 使⽤了哪些设计模式
   - ⼯⼚设计模式 : Spring 使⽤⼯⼚模式通过
     BeanFactory 、 ApplicationContext 创建 bean 对象。
   - 代理设计模式 : Spring AOP 功能的实现。
   - 单例设计模式 : Spring 中的 Bean 默认都是单例的。
   - 模板⽅法模式 : Spring 中 jdbcTemplate 、 hibernateTemplate 等以Template 结尾的对数据库操作的类，它们就使⽤到了模板模式。
   - 包装器设计模式 : 我们的项⽬需要连接多个数据库，⽽且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
   - 观察者模式: Spring 事件驱动模型就是观察者模式很经典的⼀个应⽤。
   - 适配器模式 :Spring AOP 的增强或通知(Advice)使⽤到了适配器模式、spring MVC 中也是⽤到了适配器模式适配 Controller 。
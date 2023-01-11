# OpenFegin

## 原理
 - 1、在使用Openfeign时首先会在springboot的启动类上添加@EnableFeignClients注解，该注解会导入FeignClientsRegistrar类，FeignClientsRegistrar实现了 ImportBeanDefinitionRegistrar接口, registerFeignClients函数会去扫描所有的带有@FeignClient的接口
 - 2、解析到 @FeignClient 修饰类后， Feign 框架通过扩展 Spring Bean Deifinition 的注册逻辑， 最终注册一个 FeignClientFacotoryBean 进入 Spring 容器
 - 3、Spring 容器在初始化其他用到 @FeignClient 接口的类时， 获得的是 FeignClientFacotryBean 产生的一个代理对象 Proxy.
 - 4、基于 java 原生的动态代理机制， 针对 Proxy 的调用， 都会被统一转发给 Feign 框架所定义的一个 InvocationHandler ， 由该 Handler 完成后续的 HTTP 转换， 发送， 接收， 翻译HTTP响应的工作



## 参考资料
   - [Openfeign的实现原理](https://blog.csdn.net/zzzjm_/article/details/122444219)
   - [OpenFeign调用服务的核心原理解析](https://blog.csdn.net/weixin_36488231/article/details/123570238)
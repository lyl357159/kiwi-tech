# Sentinel

## Sentinel与Hystrix对比
|对比项|Sentinel|Hystrix|
|---|---|---|
|隔离策略|信号量隔离|线程池隔离/信号量隔离
|熔断降级策略|基于响应时间或失败比率|基于失败比率
|实时指标实现|滑动窗口|滑动窗口（基于 RxJava）
|规则配置|支持多种数据源|支持多种数据源
|扩展性|多个扩展点|插件的形式
|基于注解的支持|支持|支持
|限流|基于QPS、支持基于调用关系的限流|不支持
|流量整形|支持慢启动、匀速器模式|不支持
|系统负载保护|支持|不支持
|控制台|开箱即用，可配置规则、查看秒级监控、机器发现等|不完善
|常见框架的适配|Servlet、Spring Cloud、Dubbo、gRPC 等|Servlet、Spring Cloud Netflix

## Sentinel的规则
 - **流量控制规则**
   - 其原理是监控应用流量的QPS(每秒查询率) 或并发线程数等指标，当达到指定的阈值时对流量进行控制，以避免被瞬时的流量高峰冲垮，从而保障应用的高可用性。
 - **限流降级规则**
 - **系统保护规则**
 - **来源访问控制规则**
 - **热点参数规则**

## Sentinel与Hystrix对比
- Hystrix 和 Sentinel 的实时指标数据统计实现都是基于**滑动窗口**的

## 参考资料
 - [Sentinel 原理-全解析](https://blog.csdn.net/lichao920926/article/details/105295988/)
 - [Sentinel五大规则详解](https://blog.csdn.net/weixin_44780078/article/details/128242453)
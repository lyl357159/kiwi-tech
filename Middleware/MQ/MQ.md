# MQ

## MQ的作用
  - 削峰
  - 解耦
  - 异步

## MQ的弊端
  - 系统可用性降低
  - 系统复杂度提高
  - 一致性问题
  
## 常见的MQ
 
 | |RabbitMQ	|ActiveMQ|	RocketMQ|	Kafka|
 |---|---|---|---|---|
 |公司/社区|	Rabbit|	Apache	|阿里	|Apache
 |开发语言 |Erlang	|Java	|Java	|Scala&Java
 |协议支持 |AMQP，XMPP，SMTP，STOMP	|OpenWire、AMQP、STOMP、MQTT、WS（WebSockets）和REST（Representational State Transfer,STOMPREST,XMPP|	自定义|	自定义协议，Kafka支持自己的消息协议，称为Kafka协议，同时也支持其他消息协议，如STOMP、AMQP和MQTT等。，社区封装了http协议支持
 |客户端支持语言|官方支持Erlang，Java，Ruby等,社区产出多种API，几乎支持所有语言|Java，C，C++，Python，PHP，Perl，.net等	|Java，C++（不成熟）|	官方支持Java,社区产出多种API，如PHP，Python等
 |单机吞吐量|万级(其次)|万级(最差)|十万级(最好)|十万级(次之)
 |消息延迟|微秒级|毫秒级|毫秒级|毫秒以内
 |功能特性 |并发能力强，性能极其好，延时低，社区活跃，管理界面丰富|老牌产品，成度高，文档较多|MQ功能比较完备，扩展性佳|只支持主要的MQ功能，毕竟是为大数据领域准备的。

## MQ的常见问题
  - 消息的可靠性(不丢失)
  - 消息的重复消费(及幂等性)
    > - 首先，比如 RabbitMQ、RocketMQ、Kafka，都有可能会出现消息重复消费的问题，正常。因为这问题通常不是 MQ 自己保证的，是由我们开发来保证的。
    > - 消费者意外重启可能导致重复消费
  - 消息的顺序性
  - 消息的积压
  - MQ的高可用
  
  

## 参考资料
  - [MQ的作用](https://blog.csdn.net/weixin_43915128/article/details/123613649)
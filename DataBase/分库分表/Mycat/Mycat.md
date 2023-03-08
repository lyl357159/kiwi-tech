# Mycat
  - MyCat 属于服务器端的数据库中间件，可以通过代码直连数据库，通过改写SQL分发，以保证数据的安全。它是基于Proxy，它复写了MyCat协议，将MyCat server伪装成一个 MyCat 数据库。它的这种操作，就会导致它的效率偏低，损耗略高。不过，无须修改代码，很方便！

## 参考资料
   - [分库分表神器Mycat VS ShardingSphere](https://blog.csdn.net/QQ727338622/article/details/127224609)
   - [什么是Mycat？为什么要使用MyCat?](https://blog.csdn.net/K_520_W/article/details/123702217)
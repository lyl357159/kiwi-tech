# Mybatis


## Mybatis一级缓存与二级缓存
  - MyBatis 提供了对缓存的支持，分为一级缓存和二级缓存
  - 一级缓存：SqlSession级别的缓存，缓存的数据只在SqlSession内有效。
  - 二级缓存：mapper级别的缓存，同一个namespace公用这一个缓存，所以对SqlSession是共享的，二级缓存需要我们手动开启。


## FAQ
   - **#和$的区别**
     - 使用#{}传进来的参数，mybatis默认会将其当成字符串，#{}可以有效防止sql注入，能使用#{}的地方应尽量使用#{}
       `selec * from #{table};`->`select * from "test";`
     - 使用\${}方式传入的参数，mybatis不会对它进行特殊处理。\${}则可能导致sql注入成功
       `select * from ${table};`-> `select * from test;`
 

## 参考资料
   - [mybatis中的#和$的区别](https://blog.csdn.net/zymx14/article/details/78067452)
   - [Mybatis一级缓存与二级缓存的区别你知道吗](https://bbs.huaweicloud.com/blogs/378162)

---

- [回到首页](../../README.md)

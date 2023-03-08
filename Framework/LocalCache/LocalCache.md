# LocalCache

## ehcache
  - 在Java项目广泛的使用。它是一个开源的、设计于提高在数据从RDBMS中取出来的高花费、高延迟采取的一种缓存方案。Ehcache被广泛的运用于其他的开源项目比如：hibernate。

## guava cache
  - 在GuavaCache中缓存的容器被定义为接口Cache<K, V>的实现类，这些实现类都是线程安全的，因此通常定义为一个单例。并且接口Cache是泛型，很好的支持了不同类型的key和value。


## Caffeine
  - Caffeine是使用Java8对Guava缓存的重写版本，在Spring 5.0或者Spring Boot 2.0中将取代GuavaCache，基于LRU算法实现，支持多种缓存过期策略。比如有ARC，LIRS和W-TinyLFU等都提供了接近最理想的命中率，他们这些算法进一步提高了本地缓存的效率。

---

## 参考资料
  - [本地缓存ehcache、guava cache和Caffeine](https://blog.csdn.net/weixin_42578444/article/details/80878611)

---
- [返回首页](../../README.md)
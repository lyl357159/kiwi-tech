# ElasticSearch

## 关于Elasticsearch的Master选举机制
   - 基于Bully算法
     - 对所有可以成为master的节点根据nodeId排序，每次选举每个节点都把自己所知道节点排一次序，然后选出第一个（第0位）节点，暂且认为它是master节点。
     - 如果对某个节点的投票数达到一定的值（可以成为master节点数n/2+1）并且该节点自己也选举自己，那这个节点就是master。否则重新选举。
     - 对于脑裂(brain split)问题，需要把候选master节点最小值设置为可以成为master节点数n/2+1（quorum ）


## ES写入数据的工作原理
 - 1、客户端发写数据的请求时，可以发往任意节点。这个节点就会成为coordinating node协调节点。
 - 2、计算的点文档要写入的分片：计算时就采用hash取模的方式来计算。
 - 3、协调节点就会进行路由，将请求转发给对应的primary sharding所在的datanode。
 - 4、datanode节点上的primary sharding处理请求，写入数据到索引库，并且将数据同步到对应的replica sharding
 - 5、等primary sharding 和 replica sharding都保存好文档了之后，返回客户端响应。

## ES查询数据的工作原理
 - 1、客户端发请求可发给任意节点，这个节点就成为协调节点
 - 2、协调节点将查询请求广播到每一个数据节点，这些数据节点的分片就会处理改查询请求。
 - 3、每个分片进行数据查询，将符合条件的数据放在一个队列当中，并将这些数据的文档1D、节点信息、分片信息都返回给协调节点。
 - 4、由协调节点将所有的结果进行汇总，并排序。
 - 5、协调节点向包含这些文档ID的分片发送get请求，对应的分片将文档数据返回给协调节点,最后协调节点整合数据返回给客户端。
  
## ES VS Solr
  - 当单纯地对已有数据进行检索时，solr比ES快。当实时建立索引时，solr会产生io阻塞，查询性能较差，Elasticsearch具有明显的优势。
  - 二者安装都很简单。
  - 1、solr利用zookeeper 进行分布式管理，而Elasticsearch 自身带有分布式协调管理功能。
  - 2、solr 支持更多格式的数据，比如JSON、XML、CSV，而 Elasticsearch 仅支持json文件格式。
  - 3、Solr 在传统的搜索应用中表现好于 Elasticsearch，但在处理实时搜索应用时效率明显低于 Elasticsearcho
  - 4、Solr 是传统搜索应用的有力解决方案，但 Elasticsearch更适用于新兴的实时搜索应用。

## 重要概念
  - 全文检索是指：
•通过一个程序扫描文本中的每一个单词，针对单词建立索引，并保存该单词在文本中的位置、以及出现的次数
•用户查询时，通过之前建立好的索引来查询，将秦引中单词对应的文本位置、出现的次数返回给用户，因为有了具体文本的位置，所以就可以将具体内容
读取出来了
## 参考资料
  - [elasticsearch的灵魂唯一master选举机制原理分析](https://www.jb51.net/article/245436.htm)
  - [Elasticsearch原理（五）：Master机制及脑裂分析](Elasticsearch原理（五）：Master机制及脑裂分析)
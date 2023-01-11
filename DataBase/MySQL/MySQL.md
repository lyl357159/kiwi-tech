# MySQL

## MySQL执行过程


## MySQL日志


### binlog(二进制日志)

### redo log(重做日志)

### undo log(回滚日志)

### errorlog(错误日志)

### slow query log(慢查询日志)

### general log(一般查询日志)


### relay log(中继日志)

---

## 事务的隔离级别
  ## 读未提交
   - 存在脏读(事务回滚引起)、幻读(数据增删引起)、不可重复读(数据更新引起)问题
  ## 读已提交,
   - 解决了脏读问题，存在不可重复读、幻读问题。MSSQL\Oracle默认隔离级别。
  ## 可重复读
   - 解决了不可重复读问题，存在幻读问题。MYSQL默认隔离级别。
  ## 串行化
   - 解决了以上所有问题。


## 参考资料
  - [MySQL中的几种日志了解](https://www.cnblogs.com/myseries/p/10728533.html)

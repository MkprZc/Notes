（文档：mysql的for update实现悲观锁_Go工程师体系课全新版）

## mysql  for update 实现悲观锁

条件
FOR UPDATE 仅适用于InnoDB存储引擎，且必须在事务区块(BEGIN/COMMIT)中才能生效。
mysql默认情况下每个sql都是单独的一个事务，并且是自动提交事务。



总结
select … for update; 操作

1. 未获取到数据的时候，mysql不进行锁 （no lock）
2. 获取到数据的时候，进行对约束字段进行判断，存在有索引的字段则进行row lock，否则进行 table lock


InnoDB这种行锁实现特点意味者：只有通过索引条件检索数据，InnoDB才会使用行级锁，否则，InnoDB将使用表锁！


再总结
在上述例子中 ，我们使用给name字段加索引的方法，使表锁降级为行锁，不幸的是这种方法只针对 属性值重复率低 的情况。当属性值重复率很高的时候，索引就变得低效，MySQL 也具有自动优化 SQL 的功能。低效的索引将被忽略。就会使用表锁了。



注意（待测试检测）
当使用 ‘<>’,‘like’等关键字时，进行for update操作时，mysql进行的是table lock
网上其他博客说是因为主键不明确造成的，其实并非如此；
mysql进行row lock还是table lock只取决于是否能使用索引，而 使用’<>’,'like’等操作时，索引会失效，
自然进行的是table lock；
什么情况索引会失效:
1.负向条件查询不能使用索引
负向条件有：!=、<>、not in、not exists、not like 等。
2.索引列不允许为null
单列索引不存null值，复合索引不存全为null的值，如果列允许为 null，可能会得到不符合预期的结果集。
3.避免使用or来连接条件
应该尽量避免在 where 子句中使用 or 来连接条件，因为这会导致索引失效而进行全表扫描，虽然新版的
MySQL能够命中索引，但查询优化耗费的 CPU比in多。
4.模糊查询
以上情况索引都会失效，所以进行for update的时候，会进行table lock
参考：https://juejin.im/post/5b14e0fd6fb9a01e8c5fc663
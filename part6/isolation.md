## 数据库隔离级别(以MySQL为例)

参考:
[https://zhuanlan.zhihu.com/p/76743929]

先解释名词。
脏读: 读到别的事务未提交的数据。
不可重复度: 在一次事务中两次查询的数据不一致。
幻读: 在一个事务的两次查询中数据条数不一致。

见过无数次的总结图: 


### Read Uncommitted(读取未提交数据)
读取到其他事务未提交的数据, 也被称之为脏读(DirtyRead)。

### Read Committed(读取提交内容)
两次查询的数据可能不一致。有可能别的事务在当前实物操作时更新了一条数据。

### Repeatable Read(可重复读）
无法发现其他事务新增的数据。即幻读。
Mysql官方给出的幻读解释是：只要在一个事务中，第二次select多出了row就算幻读。

### Serializable(串行化)
如果事务A开启串行化且还未提交, 则B事务提交时会被block, 等待A事务的提交或者回滚。等待超时后会有如下报错:
ERROR 1205 (HY000): Lock wait timeout exceeded; try restarting transaction
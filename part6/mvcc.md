## InnoDB的MVCC如何解决幻读

参考: 
[https://blog.csdn.net/fanghanwen\_fei/article/details/77884891]

其根本解决思路为版本控制。
InnoDB为每行记录添加了一个版本号。比如事务A操作时版本为5, 实物B操作更新版本为6并新增了数据, 实物A回身来读取时之会读取<=5的记录。这样即避免了幻读。
就是就是一个乐观锁!
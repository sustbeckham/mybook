## (高频) 谈谈对于ACID的理解?

参考:
[https://www.zhihu.com/question/31346392]
(孟波, 沈询的回答)

A(Atomic):
简单。
一个事务包含多个操作, 要么全部执行, 要么全都不执行。
沈询的总结也很好: 记录之前的版本, 允许回滚。

C(Consistency):
难理解。
事务开始和结束的中间状态不会被其他事务看到。
按照孟波的说法。一致性其实是事务整体的结果。AID都是为C服务的。

I(Isolation):
中等难理解。
如果要强一致性, 那就需要所有的事务串行执行。这对应着常说的Serilizable。
但是这性能就太差了。 于是隔离性中又开辟了如
Read Uncommitted(读取未提交数据)
Read Committed(读取提交内容)
Repeatable Read(可重复读）
沈询的总结又很好: 适当的破坏已执行来提升性能和并行度。


D(Duration):

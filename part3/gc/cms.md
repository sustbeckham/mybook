## 详述CMS的过程

参考:
[https://mp.weixin.qq.com/s/miKdBWgW5A8uEgw1\_-C9lg]
[https://blog.csdn.net/chenxinl/article/details/7980218]

### 三色法(R大也提到过)

白 -> 表示自身未被标记
灰 -> 自身标记, 但是内部应用未被处理
黑 -> 自身标记, 内部应用都被处理

###  InitialMarking(初始标记, stw)

这一过程分两步:
A. 寻找老年代中的GCRoot对象并标记
B. 寻找年轻代中存活的引用对象，引用指向老年代中的对象

这一过程结束后。老年代中只有白色和灰色对象(灰色是因为只是初始标记没有递归寻找)。

注意, 这一阶段有人说使用CMSParallelInitialMarkEnabled进行并行处理, 在JDK8之后已经是默认并行处理了, 无需手工开启。

### Marking(并发标记)
根据一阶段找出的 GCRoot 以及 跨代引用的老年代对象 开始, 递归寻找。由于这一步是和应用并行的， 所以会在标记时发生以下情况:
A. 发生YGC, 对象晋升到了老年代
B. eden空间不够. 直接分配在老年代
C. 老年代引用关系发生改变。比如老年代的A本来引用B现在变成引用C

以上情况需要重标记. 否则可能会漏标导致错误。但是如果重新标记又要重新扫描老年代那太慢了。cms的做法是把老年代分成指定大小的一个个块。利用bitmap存储。当bitmap为1代表该bit映射的区域需要重新扫描。

### Precleaning(预清理)
主要就是处理Marking阶段并发产生的新对象。重新标记这些变更。

PS: promotion我看到的确是card标记为dirty。但是引用改变和直接分配确实没有看到。

### AbortablePreclean(可中止的预清理)
做的事情和Precleaning很接近。但是本质是想等一次YGC，这样可以使得后续的CMS的Remark阶段可以更快速的结束。
默认会等5秒。但是也可以设置loop次数等其他条件让其终止。

### FinalMarking(重新标记, 也叫Remark, stw)
此时新生代的对象也会被当做此次标记的“ROOT”(哪怕这些对象已经死了).  所以才会有说加入-XX:+CMSScavengeBeforeRemark强制来一次YGC的说法来加速CMS的重标记(YGC后只有扫描一个survivor不用扫描eden当然快了)。



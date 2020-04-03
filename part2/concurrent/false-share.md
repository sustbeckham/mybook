## 关于伪共享

参考:
https://www.cnblogs.com/cyfonly/p/5800758.html
https://blog.csdn.net/qq_27680317/article/details/78486220

### 一句话解释

cpu的L1,L2,L3是以cacheline为单位存储的。当多线程修改时, 如果多个变量共享一个cacheline, 就会无意中影响彼此的性能。
  
### 一些常识

- CPU的读写速度远快于内存。所以L1L2L3(容量都很小)是为了加速用的, 相当于是CPU和内存之间的桥梁。
- L1L2是每个CPU核都有的,L3是所有核共享。
- CPU寄存器读取只需要1个cpu周期
- CPU->L1 3-4个cpu周期, 耗时约1纳秒
- CPU->L2 约10个cpu周期, 耗时约3纳秒
- CPU->L3 约40-45个cpu周期, 耗时约15纳秒
- CPU->主存, 更慢, 大约耗时60-80纳秒

### cacheline
当前系统大部分下cacheline大小都是64k。等于说能存8个long类型。

### 具体细节
当core1开始更新X, 则core2对应的缓存行更新为无效状态, 当core2取得了拥有权开始更新Y, 则core1对应的缓存行更新为无效。如此
反复, 性能损耗。 

### 如何避免

padding 
@sun.misc.Contended(需要手工添加-XX:-RestrictContended)
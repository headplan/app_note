# Redis的简介

存储介质的访问速度 : 快-&gt;慢

内存寻址-&gt;从内存中顺序读取1MB的数据-&gt;磁盘寻址-&gt;从磁盘中顺序读取1MB的数据

Redis就是一个开源的Key-Value内存存储系统 . 

Redis的特点 : 

* 全部的数据操作在内存 , 保证了高速的读写速度
* 提供丰富多样的数据类型 : string , hash , list , set , sorted set , bitmap , hyperloglog
* 提供AOP和RDB两种数据的持久化方式 , 保证了重启数据不丢失
* 所有操作都是原子性 , 还支持对几个操作合并后的原子性操作 , 即支持事务




# hash:存储对象的数据

#### 简介

hash类型很接近数据库模型 , hash的Key是个唯一值 , Value部分是个hashmap的结构 . 

#### 数据模型

| id | name | city |
| :---: | :---: | :---: |
| 5 | jeff | shenyang |

存储结构是这样的 : ![](/assets/hash2.png)key是用户id-&gt;5 , value是个hashmap , hashmap的field , 就是redis中hashmap的key就是filed , 属性名\(name , city\) , hashmap的value就是属性值\(jeff和shenyang\) , 通过key+field来操作数据 . 

如果用前面提到的string方式存储上面的数据 , 有两种方式 , 但都存在问题 : 

| 方式 | key | value | 问题 |
| :---: | :---: | :---: | :---: |
| 第一种 | 5 | {"name":"jeff","city":"sheny"} | json/反json增加开销 |
| 第二种 | 5:name | jeff | 内存开销大 |
| \# | 5:city | sheny | \# |

Redis配置中 , 这两个参数让hash更省内存 : 

* hash-max-zipmap-entries
* hash-max-zipmap-value

#### 应用场景






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

App后台常见的功能是根据用户的id获取用户的信息 , 昵称 , 头像 , 所在地等信息 . 一般高频的数据库访问 , 都会存储在Redis中 . ![](/assets/hashmap.png)获取了用户ID后需要获取用户的数据 , 用hgetall命令获取id下所有的field和value : 

```
hgetall id
```

> 注 : 修改了数据库的用户数据 , 也要把这些数据同步更新到Redis , 用户来防止Redis和数据库的数据不一致 .




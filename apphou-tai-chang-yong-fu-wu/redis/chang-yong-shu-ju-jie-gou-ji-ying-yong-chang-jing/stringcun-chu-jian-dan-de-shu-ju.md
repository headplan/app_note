# string:存储简单的数据

#### 简介

最基本的数据类型 , 在Redis中是二进制安全 . 接收任何格式的二进制数据 , 例如图片或者json的字符串 . 在Redis中字符串最多可以容纳的数据长度是512MB . 

#### 数据模型

基本的Key-Value结构 , Key可以看做某个数据在Redis中的唯一标识 , Value是具体的数据 . 

Key-&gt;Value

"name"-&gt;"jeff"

"city"-&gt;"shenyang"

#### 应用场景

String类型灵活 , 存储量大 . 在App后台中常用来缓存数据 . 例如常见的商品分类栏 , 这类界面的特点是 , 访问频率高 , 数据不经常变动 . 所以放在一个Key-Value结构中 , App后台从这个Key中读取数据 , 当发生变化时 , 用新的数据直接覆盖这个Key即可 . 

"category"-&gt;{"常用分类":...,"潮流女装":...}

要获取分类数据时 , 直接get category即可 . 

一般App端为了在网络不可用的时候也有良好的用户体验 , 会在App本地也缓存一份数据 , 整个流程是 : 

![](/assets/category.png)




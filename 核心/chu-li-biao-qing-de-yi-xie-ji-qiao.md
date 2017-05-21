# 处理表情的一些技巧

#### 表情在MySQL中的存储

* 升级MySQL到5.5以上 , 字符编码改为utf8mb4\_general\_ci
* 在MySQL5.1中存储表情的方法 , 把含有表情数据的字段类型变为blob , 二进制存储 , 这是改动最少的情况 . 

#### 当文字中夹带表情的处理

应用场景

* 用户昵称带有表情
* APNS推送的文字中带表情

使用类库替换文字中的表情

> https://github.com/iamcal/PHP-emoji
>
> https://github.com/newjueqi/converemojitostr




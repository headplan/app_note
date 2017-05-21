# App通信安全

#### URL签名

不在网络上传输token的方案成为URL签名 .

URL签名过程 :

1. App后台中用户名和密码验证正确后 , 把token字符串和用户信息返回给app客户端 . Redis中除了维护token字符串和用户信息的对应关系 , 还要维护用户id和token的对应关系 , 以方便后面App后台验证URL签名时快速查找 . 
2. 假设API请求为test.com/user/info , App用token字符串"dh2423tewf"和URL的md5值作为URL签名sign  
   sign=md5\("test.com/user/info?token=dh2423tewf"\);  
   这样API请求加上URL签名sign和用户id , 就不用token附在URL上了.

3. App后台接收到这个URL后 , 用上一步算法生成标签和sign参数对比 , 如果发现相等 , 就表示验证通过 , 继续执行这个API的其他逻辑

URL签名的验证流程

![](/assets/urlqianming.png)

上面的签名方法有一个问题 , 没有过期时间 , 如果这个API请求的URL被截获 , 就能反复调用 .

改进方法是在传递的参数中增加时间戳 , 判断URL是否已经失效 .

但是还有一个问题 , App端的时间和App后台的时间不一致怎么办 ? 保证时间同步的方法是 , App启动时通过API获取APP后台的时间 , 保存App端时间和App后台的时间差 , 用App端的这个时间差调整生成的时间戳 .

例如 :

某一时刻API获取APP后台的时间是aaa , App端时间是bbb , 两者之间的差是3 . 当App本地时间aaa向App后台发送请求时 , 加上时间差3 , 即是参数传递中的时间戳 .

现在 , 我们来改进一下流程 .

1. 假设API请求是"test.com/user/info" , 用token字符串和时间戳生成md5值的URL签名sign  
   API的请求路径加上URL签名sign , 用户标识userId和API发送时的时间戳 .

2. App后台接收到这个API请求 , 如果收到该API请求的时间和URL上时间戳的值 , 超过了30s , 可能意味着URL被反复调用了 , 应拒绝继续访问 . 如果App后台认为时间合法 , 那就继续判断sign是否一致 .

但是 , 还有几个问题 .

* 用户登录后 , App后台返回给app的信息是没有加密的
* URL签名只能保护token值不泄露 , 但没法保护其他敏感信息泄露
* URL被拦截后一段时间还是能被重复调用的

下面继续讨论解决方案 .

#### AES对称加密

同一个密钥可以同时用作信息的加密和解密 , 这种加密方法称之为对称加密 , 也称为单密钥加密 . 格式如下

```
AES加密(data, secretKey)
```

**加密App后台返回的token数据**

URL签名中的一个缺点是 , 比如登录后的个人信息是明文的 : 

```
{"userID":5, "name":"headplan", "token":"dafasdfsdafewr"}
```

后台返回给App用户个人信息的流程是 : 

1. 用户在App登录得到个人信息
   ```
   {"userID":5, "name":"headplan", "token":"dafasdfsdafewr"}
   ```
2. 根据App后台当前的时间戳 , 生成HTTP请求头

   ```
   Token-Param: 14234232422
   ```

3. 把请求头Token-Param的22位长度作为密钥secretKey

4. 用AES算法把个人信息用密钥secretKey加密 , 再进行base64编码 , 最后用HTTPS协议返回给App . 

此时HTTPS协议内容为 : 

* HTTP URL
* HTTP请求方式
* HTTP请求头
* HTTP body - Base64Encode\(AES加密\(个人信息, 请求头Token-Param的22位长度\)\)




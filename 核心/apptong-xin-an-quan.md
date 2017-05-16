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




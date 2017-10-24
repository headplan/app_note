# RESTful API 设计指南

网络应用程序，分为前端和后端两个部分。当前的发展趋势，就是前端设备层出不穷（手机、平板、桌面电脑、其他专用设备......）。

因此，必须有一种统一的机制，方便不同的前端设备与后端进行通信。这导致API构架的流行，甚至出现["API First"](http://www.google.com.hk/search?q=API+first)的设计思想。[RESTful API](http://en.wikipedia.org/wiki/Representational_state_transfer)是目前比较成熟的一套互联网应用程序的API设计理论。

介绍RESTful API的设计细节，探讨如何设计一套合理、好用的API , 参考下面两篇文章 :

> [https://codeplanet.io/principles-good-restful-api-design/](https://codeplanet.io/principles-good-restful-api-design/)
>
> [https://bourgeois.me/rest/](https://bourgeois.me/rest/)

#### 协议

API与用户的通信协议，总是使用HTTPs协议。

#### 域名

应该尽量将API部署在专用域名之下。

```
https://api.example.com
```

如果确定API很简单，不会有进一步扩展，可以考虑放在主域名下。

```
https://example.org/api/
```

#### 版本

应该将API的版本号放入URL。

```
https://api.example.com/v1/
```

另一种做法是，将版本号放在HTTP头信息中，但不如放入URL方便和直观。[Github](https://developer.github.com/v3/media/#request-specific-version)采用这种做法。但RESTful建议的是前一种方式。

#### 路径

路径又称"终点"（endpoint），表示API的具体网址。就是我们常说的路由。

在RESTful架构中，每个网址代表一种资源（resource），所以网址中不能有动词，只能有名词，而且所用的名词往往与数据库的表格名对应。一般来说，数据库中的表都是同种记录的"集合"（collection），所以API中的名词也应该使用复数。

举例来说，有一个API提供动物园（zoo）的信息，还包括各种动物和雇员的信息，则它的路径应该设计成下面这样。

```
https://api.example.com/v1/zoos
https://api.example.com/v1/animals
https://api.example.com/v1/employees
```

HTTP动词

过滤信息

状态码

错误处理

返回结果

Hypermedia API

其他

> 相关参考
>
> [http://www.ruanyifeng.com/blog/2014/05/restful\_api.html](http://www.ruanyifeng.com/blog/2014/05/restful_api.html)
>
> 实际应用例子参考Laravel Note中的API开发文档




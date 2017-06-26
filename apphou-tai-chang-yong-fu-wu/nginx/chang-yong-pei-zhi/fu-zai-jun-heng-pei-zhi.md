# 负载均衡配置

```
upstream test.com {
    server 192.168.1.20:80 weight=2;
    server 192.168.1.21:80 weight=1;
}
```

upstream模块通过简单的调度算法实现客户端到服务器的负载均衡 . 上面的例子中 , test.com是这个负载均衡的名字 , 可以在后面的配置中调用 . 

Nginx支持一下4种负载均衡算法 : 

* 加权轮询\(默认的算法\) - 请求按时间分别分配到不同的服务器上 . 
* ip\_hash - 使用请求的ip算出hash值 , 根据hash值分配到不同的服务器上 , 固定ip的请求 , 会分配到固定的服务器 . 这种策略有效的解决了网站服务的session共享问题 . 
* fair - 按后端服务器的响应时间来分配请求 , 响应时间短的优先分配 . Nginx默认是不支持这种负载均衡算法的 , 需要安装Nginx模块和upstream\_fair模块 . 
* url\_hash - 使用请求的URL算出hash值 , 根据hash值分配到不同的服务器上 , 固定的URL请求 , 会分配到固定的服务器上 . 这种策略有利于提高后端服务器的缓存命中率 . Nginx默认是不支持这种负载均衡算法的 , 需要安装Nginx的hash软件包 . 

upstream模块可以为所配置的服务器指定状态值 , 常见的有 : 

* down - 服务器不参与到负载均衡中 , 当后台人员进行故障排查时这个状态非常有用 . 
* weight - 制定轮询的权重 , 权重越大分配到的几率越多 . 前面例子中分配的请求比例就是2:1 . 
* backup - 备份机器 . 当其他的服务器不可用时 , 才把请求分配到这台服务器上 . 
* max\_fails - 允许请求失败的次数 , 默认值是1 . 
* fail\_timeout - 经历了前面max\_fails次失败后 , 暂停服务的时间

> 注意 : 当负载均衡是ip\_hash算法时 , 服务器的状态值不能是backup和weight .



# Nginx反向代理

http proxy模块中的相关指令 : 

```
proxy_connect_timeout 300s; # 后端服务器连接的超时时间_发起握手等候响应超时时间
proxy_send_timeout 900; # 后端服务器数据回传时间_就是在规定时间之内后端服务器必须传完所有的数据
proxy_read_timeout 900; # 连接成功后_等候后端服务器响应时间_其实已经进入后端的排队之中等候处理（也可以说是后端服务器处理请求的时间）
proxy_buffer_size 32k;
proxy_buffers 4 64k;
proxy_busy_buffers_size 128k;
proxy_redirect off;
proxy_hide_header Vary;
# 允许重新定义或者添加发往后端服务器的请求头
# proxy_set_header
proxy_set_header Accept-Encoding ''; # 如果某个请求头的值为空，那么这个请求头将不会传送给后端服务器
proxy_set_header Referer $http_referer;
proxy_set_header Cookie $http_cookie;
proxy_set_header Host $host:$server_port;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```

**反向代理样例**

```
upstream test {
     server 10.46.194.17:8088 weight=5;
     server 10.46.192.41:8080 weight=5;
}

server {
     listen 8079;
     server_name www.test.com;
     underscores_in_headers on;
     ignore_invalid_headers off;
     
     location / {
     93             proxy_set_header Host $host;
     94             proxy_set_header X-Real-IP $remote_addr;
     95             proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
     96             proxy_pass http://qss;
     97         }
```

**这里记录两个自定义header头的参数**

```
underscores_in_headers on; # 开启配置这个后,HTTP头部有下划线的,
ignore_invalid_headers off; 
# 是否忽略不合法的http首部.默认为on,off意味着请求首部中出现不合规的首部将拒绝响应.只能用于server和http中,建议改为off.
```

1 . nginx是支持读取非nginx标准的用户自定义header的 , 但是需要在http或者server下开启header的下划线支持 : 

* underscores\_in\_headers on;

2 . 比如我们自定义header为X-Real-IP,通过第二个nginx获取该header时需要这样

* $http\_x\_real\_ip; \(一律采用小写，而且前面多了个http\_\)

3 . 如果需要把自定义header传递到下一个nginx

* 如果是在nginx中自定义采用proxy\_set\_header X\_CUSTOM\_HEADER $http\_host;
* 如果是在用户请求时自定义的header，例如curl –head -H “X\_CUSTOM\_HEADER: foo” [http://domain.com/api/test](http://domain.com/api/test)
  则需要通过`proxy_pass_header X_CUSTOM_HEADER`来传递




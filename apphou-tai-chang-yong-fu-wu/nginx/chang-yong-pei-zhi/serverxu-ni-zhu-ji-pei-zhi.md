# server虚拟主机配置

```
server {
    listen 80;
    server_name localhost test.com;
    index index.html
    root /var/www/test;
}
```

配置的含义 :

* listen - 指定虚拟主机监听的端口
* server\_name - 指定虚拟主机对应的域名 , 多个域名之间以空格分割
* index - 默认的首页支持
* root - 网站的目录

**配置补充**

```
server {
    listen 80;
    server_name localhost test.com;
    index index.html
    root /var/www/test;
    
    # 日志格式设定
    log_format access '$remote_addr - $remote_user [$time_local] "$request" '
        '$status $body_bytes_sent "$http_referer" '
        '"$http_user_agent" $http_x_forwarded_for';
        
    #定义本虚拟主机的访问日志
    access_log  /usr/local/nginx/logs/host.access.log  main;
    access_log  /usr/local/nginx/logs/host.access.404.log  log404;
    
    # 错误显示页面
    error_page 500 502 503 504 /50x.html
}
```




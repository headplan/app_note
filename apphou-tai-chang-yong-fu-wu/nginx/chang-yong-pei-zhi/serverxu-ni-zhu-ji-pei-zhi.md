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








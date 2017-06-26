# 常用location配置

**PHP常用配置**

```
# 防盗链配置
location ~ .*\.(wma|wmv|asf|mp3|mmf|zip|rar|jpg|gif|png|swf|flv|mp4)$ {
    valid_referers none blocked *.test.com www.test.com;
    if ($invalid_referer) {
            rewrite ^/ /403.html;
            return 403;
    }
}
# 转发动态请求到后端应用服务器
# 这里直接走内存里的unix:/dev/shm/php-cgi.sock;当然还要修改php-fpm.conf的配置.
# 引入了fastcgi.conf配置,所以这里就不用添加下面这个配置了
# fastcgi_param SCRIPT_FILENAME $fastcgi_script_name
location ~ [^/]\.php(/|$) {
    #fastcgi_pass remote_php_ip:9000;
    fastcgi_pass unix:/dev/shm/php-cgi.sock;
    fastcgi_index index.php;
    include fastcgi.conf;
}
# 处理静态文件请求
# 关闭了日志记录
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
    expires 30d;
    access_log off;
}
# 处理静态文件请求
# 关闭了日志记录
location ~ .*\.(js|css)?$ {
    expires 7d;
    access_log off;
}
# 禁止htaccess
location ~ /\.ht {
    deny all;
}
```

**开启性能统计**

```
# 编译源码时带上参数"--with-http_stub_status_module",就安装了Nginx的统计模块
# 配置,其中allow只允许本机访问,deny禁用所有,可以注释打开
location /nginx_status {
    stub_status on;
    access_log off;
    allow 127.0.0.1;
    deny all;
}
# 访问显示内容
Active connections: 1 
server accepts handled requests
 598 598 578 
Reading: 0 Writing: 1 Waiting: 0 

Active connections - 当前nginx正在处理的活动链接数
server accepts handled requests - 共处理了598次连接,598次握手,578次请求
Reading - 读取到客户端的header信息数
writing - 返回给客户端的header信息数
waiting - 开启keep-alive的情况下,这个值等于Active-(Reading+Writing),是已经处理完成,等待下次请求指令的驻留连接.
当快速请求完毕时,Waiting数比较多是正常的;Reading+Writing比较多,则说明后台并发访问量较大,正在处理过程中.

```




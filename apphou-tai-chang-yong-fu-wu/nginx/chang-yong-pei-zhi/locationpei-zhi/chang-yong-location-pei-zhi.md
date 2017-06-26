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
# 这里直接走内存里的unix:/dev/shm/php-cgi.sock;
# 引入了fastcgi.conf配置
location ~ [^/]\.php(/|$) {
	#fastcgi_pass remote_php_ip:9000;
	fastcgi_pass unix:/dev/shm/php-cgi.sock;
	fastcgi_index index.php;
	include fastcgi.conf;
}
# 处理静态文件请求
location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
	expires 30d;
	access_log off;
}
# 处理静态文件请求
location ~ .*\.(js|css)?$ {
	expires 7d;
	access_log off;
}
# 禁止htaccess
location ~ /\.ht {
	deny all;
}
```




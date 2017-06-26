# 常用location配置

```
# 防盗链配置
location ~ .*\.(wma|wmv|asf|mp3|mmf|zip|rar|jpg|gif|png|swf|flv|mp4)$ {
	valid_referers none blocked *.test.com www.test.com;
	if ($invalid_referer) {
			rewrite ^/ /403.html;
			return 403;
	}
}
```




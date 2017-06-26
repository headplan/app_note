# 常用Rewrite规则

**规则片段**

```
多目录转成参数
abc.domian.com/sort/2 => abc.domian.com/index.php?act=sort&name=abc&id=2
1.     if ($host ~* (.*)\.domain\.com) {
2.     set $sub_name $1;   
3.     rewrite ^/sort\/(\d+)\/?$ /index.php?act=sort&cid=$sub_name&id=$1 last;
4.     }
目录对换
/123456/xxxx -> /xxxx?id=123456
1.     rewrite ^/(\d+)/(.+)/ /$2?id=$1 last;
例如下面设定nginx在用户使用ie的使用重定向到/nginx-ie目录下：
1.     if ($http_user_agent ~ MSIE) {
2.     rewrite ^(.*)$ /nginx-ie/$1 break;
3.     }
目录自动加“/”
1.     if (-d $request_filename){
2.     rewrite ^/(.*)([^/])$ http://$host/$1$2/ permanent;
3.     }

将多级目录下的文件转成一个文件，增强seo效果
/job-123-456-789.html 指向/job/123/456/789.html
1.     rewrite ^/job-([0-9]+)-([0-9]+)-([0-9]+)\.html$ /job/$1/$2/jobshow_$3.html last;
将根目录下某个文件夹指向2级目录
如/shanghaijob/ 指向 /area/shanghai/
如果你将last改成permanent，那么浏览器地址栏显是 /location/shanghai/
1.     rewrite ^/([0-9a-z]+)job/(.*)$ /area/$1/$2 last;
上面例子有个问题是访问/shanghai 时将不会匹配
1.     rewrite ^/([0-9a-z]+)job$ /area/$1/ last;
2.     rewrite ^/([0-9a-z]+)job/(.*)$ /area/$1/$2 last;
这样/shanghai 也可以访问了，但页面中的相对链接无法使用，
如./list_1.html真实地址是/area /shanghia/list_1.html会变成/list_1.html,导至无法访问。
那我加上自动跳转也是不行咯
(-d $request_filename)它有个条件是必需为真实目录，而我的rewrite不是的，所以没有效果
1.     if (-d $request_filename){
2.     rewrite ^/(.*)([^/])$ http://$host/$1$2/ permanent;
3.     }
知道原因后就好办了，让我手动跳转吧
1.     rewrite ^/([0-9a-z]+)job$ /$1job/ permanent;
2.     rewrite ^/([0-9a-z]+)job/(.*)$ /area/$1/$2 last;
文件和目录不存在的时候重定向：
1.     if (!-e $request_filename) {
2.     proxy_pass http://127.0.0.1;
3.     }
```

**框架网站使用**




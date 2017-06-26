# location配置

location支持正则表达式和条件判断匹配 , 用户可以通过location指令对动态,静态网页进行过滤 . 

例如 : 

```
location ~ .*\.(git|jpg|jpeg|png)$
{
    expires 30d;
}
```

这段location的配置的含义是 : 经过正则表达式匹配 , 设置文件格式为GIT,JPEG,PNG的文件在HTTP应答中"Expires"和"Cache-Control"的HTTP头 , 以达到在浏览器中缓存图片的作用 , 大概意思就是把图片在浏览器中缓存30天 . 






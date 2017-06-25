# fastcgi配置

#### fastcgi\_params 与 fastcgi.conf

有了fastcgi\_params为什么还要fastcgi.conf文件 .

```
diff fastcgi.conf fastcgi_params
# fastcgi.conf多了一行
fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
```

历史原因 : fastcgi.conf是nginx 0.8.30 \(released: 15th of December 2009\)才引入的 . 因为人们经常在配置fastcgi\_param的SCRIPT\_FILENAME选项时 , 总使用硬编码 , 就是写死 . 为了规范才引入的 .

> 不过这样的话就产生一个疑问：
>
> 为什么一定要引入一个新的配置文件 , 而不是修改旧的配置文件 ?
>
> 这是因为fastcgi\_param指令是数组型的 ,
>
> 和普通指令相同的是 : 内层替换外层 ;
>
> 和普通指令不同的是 : 当在同级多次使用的时候 , 是新增而不是替换 .

所以新的引入方式一般为 :

```
include fastcgi.conf;
```

---

#### 以PHP为例

```
fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
```

这句话其实就是定义php中用到的服务器变量 ——$\_SERVER . 

服务器向处理php的cgi传递过去他需要的一些参数 , 而至少要有下面的两个参数php才能执行起来 : 

```
fastcgi_param SCRIPT_FILENAME /home/www/scripts/php$fastcgi_script_name;
fastcgi_param QUERY_STRING $query_string; 
```

所以我们在没有定义SCRIPT\_FILENAME这个系统变量的时候PHP是没法解释执行的 . 这个变量的定义可以写在nginx的配置文件nginx.conf里也可以写在外部 用include的方式在nginx.conf里包含进来 . 


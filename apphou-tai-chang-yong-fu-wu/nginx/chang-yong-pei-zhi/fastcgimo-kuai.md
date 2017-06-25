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

#### fastcgi.conf

```
fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;#脚本文件请求的路径  
fastcgi_param  QUERY_STRING       $query_string; #请求的参数;如?app=123  
fastcgi_param  REQUEST_METHOD     $request_method; #请求的动作(GET,POST)  
fastcgi_param  CONTENT_TYPE       $content_type; #请求头中的Content-Type字段  
fastcgi_param  CONTENT_LENGTH     $content_length; #请求头中的Content-length字段。  
  
fastcgi_param  SCRIPT_NAME        $fastcgi_script_name; #脚本名称   
fastcgi_param  REQUEST_URI        $request_uri; #请求的地址不带参数  
fastcgi_param  DOCUMENT_URI       $document_uri; #与$uri相同。   
fastcgi_param  DOCUMENT_ROOT      $document_root; #网站的根目录。在server配置中root指令中指定的值   
fastcgi_param  SERVER_PROTOCOL    $server_protocol; #请求使用的协议，通常是HTTP/1.0或HTTP/1.1。    
  
fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;#cgi 版本  
fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;#nginx 版本号，可修改、隐藏  
  
fastcgi_param  REMOTE_ADDR        $remote_addr; #客户端IP  
fastcgi_param  REMOTE_PORT        $remote_port; #客户端端口  
fastcgi_param  SERVER_ADDR        $server_addr; #服务器IP地址  
fastcgi_param  SERVER_PORT        $server_port; #服务器端口  
fastcgi_param  SERVER_NAME        $server_name; #服务器名，域名在server配置中指定的server_name  
  
# PHP only, required if PHP was built with --enable-force-cgi-redirect  
fastcgi_param  REDIRECT_STATUS    200;  

# fastcgi_param  PATH_INFO        $path_info;#可自定义变量  
  
# 在php可打印出上面的服务环境变量
# 如:echo $_SERVER['REMOTE_ADDR']
# 也开始使用PHP_VALUE限制文件打开路径,但存在弊端,也可在php.ini中配置
# 提高安全,但影响速度,参考http://blog.csdn.net/fdipzone/article/details/54562656
# fastcgi_param  PHP_VALUE  "open_basedir=...";
```




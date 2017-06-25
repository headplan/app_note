# 常用配置

Nginx的配置文件nginx.conf是纯文本文件 , 位于Nginx安装目录的conf目录下 , 整个配置文件是以块的形式组织 , 每个块以{}来表示 . 采用嵌套的方式 , 一个大块中可以包括小块 . 最大的块是main块 , main块里包含event块和http块 , http块包含了upstream块和server块 , server块包含了多个location块 .

* main - Nginx的全局属性配置
* event - Nginx的工作模式及连接数上线
* http - http服务器相关属性的配置
* upstream - 负载均衡属性的配置
* server - 虚拟主机的配置
* location - location的配置

![](/assets/nginx.png)

#### Nginx的全局配置

```
user www www;
worker_processes 4;
error_log /home/wwwlogs/nginx_error.log crit;
pid /usr/local/nginx/logs/nginx.pid;
worker_rlimit_nofile 52000;
```

* user - 指定了Nginx工作进程运行的用户及用户组 , 默认是nobody , 这里配置的是用户www和用户组www
* worker\_processes - 指定Nginx开启的工作进程数 . 每个进程大约占用10-12MB的内存 . 如果是多核的CPU , 这里应设置和CPU核数一样的进程数 . 
* error\_log - 全局错误日志的位置与日志输出的级别 . 日志的输出级别可选择 . 其中debug级别输出的日志最详细 . 
  * debug
  * info
  * notice
  * warn
  * error
  * crit
* pid - 存储Nginx进程id的文件路径
* `worker_rlimit_nofile`- 指定了一个Nginx进程最多可以打开的文件描述符 . 这里的配置受限于Linux中的那个配置 , 前面我们提到过 , 就是Too many open files那个错误 .  

#### event配置

```
events
{
    use epoll;
    work_connections 51200;
    multi_accept on;
}
```

* use - 指定Nginx的工作模式 , 前面已经提过 , epoll是首选 , FreeBSD下kqueue是首选
  * select
  * poll
  * kqueue
  * epoll
  * rtsig
  * /dev/poll
* worker\__connections - 定义每个worker pricess的最大连接数 , 默认1024 . 这里的配置也受限于Linux中最多可以打开的文件描述符数限制 . 所以Nginx可以处理的最大连接数为max_\_clients = worker\_processes × worker\_connections
* multi\_accept - 设置Nginx是否允许,在已经得到一个新连接的通知时,接收尽可能更多的连接 . 

#### http配置

```
http
{
    include mime.types;
    default_type Application/octet-stream;
    client_header_buffer_size 32k;
    large_client_header_buffers 4 32k;
    client_max_body_size 50m;
    sendfile on;
    tcp_nopush on;
    keepalive_timeout 60;
    tcp_nodelay on;

    gzip on;
    gzip_min_length 1k;
    gzip_buffers 4 16k;
    gzip_http_version 1.0;
    gzip_comp_level 2;
    gzip_types text/plan Application/x-javascript text/css Application/xml;
    gzip_vary on;

    include vhost/*.conf;
}
```

* include - 包含其他的配置文件 , 一般是mime.types一个文件类型的列表 , 和nginx.conf配置文件同级目录下
* default\_type - 默认类型application/octet-stream;二进制流格式 . 意思是当文件类型未定义时 , 使用二进制流的格式.
* server\_names\_hash\_bucket\_size - 服务器名字的hash表大小 . 一般设置一个很长的域名之后会提示修改这个配置大小的错误提示
* > 保存服务器名字的hash表是由指令server\_names\_hash\_max\_size 和server\_names\_hash\_bucket\_size所控制的。参数hash bucket size总是等于hash表的大小，并且是一路处理器缓存大小的倍数。在减少了在内存中的存取次数后，使在处理器中加速查找hash表键值成为可能。如果hash bucket size等于一路处理器缓存的大小，那么在查找键的时候，最坏的情况下在内存中查找的次数为2。第一次是确定存储单元的地址，第二次是在存储单元中查找键值。因此，如果Nginx给出需要增大hash max size 或 hash bucket size的提示，那么首要的是增大前一个参数的大小.
* client\_header\_buffer\_size - 客户端请求头buffersize的大小
* large\_client\_header\_buffer - 客户端请求中较大的消息头的缓存的数量和大小 , 这里第一个参数是数量 , 第二个参数是大小
* client\_max\_body\_size - 客户端请求中http body的大小 , 一般可以理解为请求的文件大小 . 
* client\_body\_buffer\_size - 配置上一个配置的缓冲器大小 , 可以理解为上面的配置的分页大小
* sendfile - 是否启动高效传输文件的模式 . 
* > sendfile可以让Nginx在传输文件时直接在磁盘和tcp Socket之间传输数据 . 如果这个参数不开启 , 会先在用户空间申请一个buffer缓冲器 , 用read函数把数据从磁盘读到cache , 再从cache读取到用户空间的buffer , 再用write函数把数据从用户空间的buffer写入到内核的buffer , 最后到TCP Socket
  >  . 开始这个参数就可以让数据不用经过用户buffer了 . ![](/assets/sendfile.png)

#### 

#### 负载均衡配置

#### server虚拟主机配置

#### location配置

#### HTTPS的配置

#### 下载App的配置

#### 生产环境中修改配置的良好习惯




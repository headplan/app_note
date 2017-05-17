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

#### http配置

#### 负载均衡配置

#### server虚拟主机配置

#### location配置

#### HTTPS的配置

#### 下载App的配置

#### 生产环境中修改配置的良好习惯




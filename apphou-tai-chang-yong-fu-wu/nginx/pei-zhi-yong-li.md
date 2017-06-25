# 配置用例

**nginx.conf全局配置文件**

```
user www www; # 工作进程运行的用户及用户组
worker_processes auto; # 开启的工作进程数
error_log logs/error.log crit; # 全局错误日志输出位置以及级别
pid logs/nginx.pid # 存储Nginx进程id的文件路径
worker_rlimit_nofile 51200; # 一个进程打开文件数限制,但受Linux的此参数限制

# 工作模式及连接数上线
events {
  use epoll; # 工作模式,如果是MacOS使用kqueue模式
  worker_connections 51200; # 每个进程最大连接数(进程就是worker_processes)
  multi_accept on; 设置Nginx是否允许,在已经得到一个新连接的通知时,接收尽可能更多的连接
}

# http配置
http {
  include mime.types; # 文件类型
  default_type application/octet-stream; # 默认二进制流

  # 服务器客户大小以及缓冲器大小配置
  server_names_hash_bucket_size 128;
  client_header_buffer_size 32k;
  large_client_header_buffers 4 32k;
  client_max_body_size 1024m;
  client_body_buffer_size 10m;

  # 高效传输文件模式
  sendfile on;
  tcp_nopush on;
  tcp_nodelay on;
  keepalive_timeout 60;
  server_tokens off;
  
  # fastcgi模块
  # gzip模块
  
  # 静态文件缓存配置
  
  # ===== Defaule Server =====
  server {
  
  }
  
  # ===== vhost =====
  include vhost/*.conf;
}
```




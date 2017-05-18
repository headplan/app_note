# 配置文件详解

配置文件位于/etc/my.cnf

```
max_connections = 1000
max_connect_errors = 50
key_buffer_size = 3M
max_allowed_packet = 16M
thread_cache_size = 300
thread_concurrency = 8
sort_buffer_size = 1M
join_buffer_size = 8M
query_cache_size = 512M
query_cache_limit = 2M
read_buffer_size = 2M
read_rnd_buffer_size = 16M
myisam_sort_buffer_size = 16M
innodb_buffer_pool_size = 4G
innodb_log_file_size = 128M
innodb_log_buffer_size = 8M
innodb_flush_log_at_trx_commit = 1
innodb_lock_wait_timeout = 50
```

* max\_connections - MySQL所允许的同时会话数的上限 . 即使到了最大连接数的上限 , 其中一个连接也会被保留作为管理员登录用  . 如果该数值设置过少 , 很容易出现错误"Too many connections" . 
* max\_connect\_errors - 每个客户端连接的最大错误允许数 , 如果达到了这个数的限制 , 这个客户端将会被MySQL服务阻止 , 直到执行"FLUSH HOSTS"或者服务重启 . 
* key\_buffer\_size - 关键词缓冲区大小 , 用来缓冲MyISAM表的索引块 , 它决定了数据库索引处理的速度 , 尤其是读取索引的速度 . 
* max\_allowed\_packet - 设置最大包 , 限制server接受的数据包大小 , 避免超长SQL的执行有问题 . 当MySQL客户端或mysqld服务器收到大




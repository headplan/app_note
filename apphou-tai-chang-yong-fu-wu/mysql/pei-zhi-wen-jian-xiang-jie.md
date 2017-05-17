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




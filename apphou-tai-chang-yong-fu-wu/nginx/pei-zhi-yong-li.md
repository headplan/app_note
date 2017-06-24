# 配置用例

**nginx.conf全局配置文件**

```
user www www; # 工作进程运行的用户及用户组
worker_processes auto; # 开启的工作进程数
error_log logs/error.log crit; # 全局错误日志输出位置以及级别
worker_rlimit_nofile 51200; # 一个进程打开文件数限制,但受Linux的此参数限制

# 
```




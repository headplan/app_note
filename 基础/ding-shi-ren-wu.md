# 定时任务

#### Linux定时任务Crontab

```
crontab [-u username] [-l|-e|-r]
参数:
-u : 只有root才能进行这个任务 , 编辑某个用户的crontab
-e : 编辑crontab的工作内容
-l : 查阅crontab的工作内容
-r : 移除所有的crontab的工作内容
```

定时任务写到/var/spool/cron/ , 写入到用户的文件夹中/var/spool/cron/headplan

命令格式

分 时 日 月 周 命令

特殊符号

```
* 任何时刻都接受
, 表示多个时间段 例, 2,3 * * * * cmd 表示第二分钟和第四分钟都执行cmd命令
- 表示时间间隔 例, 2-5 * * * * cmd 表示从第二分钟开始到第五分钟 , 每分钟都执行cmd命令
/n 表示隔n个时间单位 例, */5 * * * * cmd 表示每隔5分钟执行cmd命令
```

crontab命令是分级的 , 要执行秒级命令 , 可以这样写

```
***** cmd
***** sleep 20; cmd
***** sleep 40; cmd
```




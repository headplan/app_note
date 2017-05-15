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
# 每20秒执行一次命令
* * * * * cmd
* * * * * sleep 20; cmd
* * * * * sleep 40; cmd
```

#### 定时任务管理工具

Java下的Quartz

Python下的APScheduler , 他是基于上面的Python写的定时框架 , 实现了Quartz的所有功能 .

* 通过RAM,MySQL,MongoDB文件 , 持久化存储定时任务
* 支持秒级定时任务
* 支持日期 , 固定时间间隔和Crontab类型的定时任务

**APScheduler安装**

使用easy\_install安装 :

```
easy_install apscheduler
```

源码安装 :

```
Python setup.py install
```

创建一个定时三秒运行一次的定时任务 : 

```
from datetime import datetime
import time
from apscheduler.scheduler import Scheduler

def tick();
    print('Tick! The time is: %s' % datetime.now())
    
if __name__ == '__main__':
    scheduler = Scheduler()
    scheduler.add_interval_job(tick, seconds=3)
    print('Press Ctrl+C to exit')
    scheduler.start()
    while True:
        print('This is the main thread.')
        time.sleep(2)
```




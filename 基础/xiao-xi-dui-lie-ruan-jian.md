# 消息队列软件

消息队列就是把一个大任务拆分成若干小任务 , 其中先完成核心的小任务 , 那些不影响整体进度的任务 , 排队交给消息队列完成 . 

App后台中发送邮件 , 发送短信 , 消息推送等任务都非常适合在消息队列中处理 . 

#### 消息队列的工作流程

* 队列服务端
* 队列生产者
* 队列消费者

场景 : 生产线上 , 工人a不断把产品放在传输带上 , 传输带传输产品到终点 , 工人b把产品取出加工 . 

App后台把消息推入消息队列 , 即生产者 . 

守护进程 , 队列消费者不断检测消息队列中有没有没新的消息 . 

消息队列软件即服务端 . 

#### ![](/assets/queue.png)常见的消息队列产品

* RabbitMQ - 企业级重量级产品
* Redis - 轻量级消息队列产品
* ZeroMQ - 号称最快的消息队列产品 , 针对大吞吐量的需求场景
* ActiveMQ - 是Apache软件基金会下的一个子项目




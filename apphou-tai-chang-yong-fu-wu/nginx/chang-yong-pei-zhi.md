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

#### event配置

#### http配置

#### 负载均衡配置

#### server虚拟主机配置

#### location配置

#### HTTPS的配置

#### 下载App的配置

#### 生产环境中修改配置的良好习惯




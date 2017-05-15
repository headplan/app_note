# RPC

RPC远程调用框架 - Remote Procedure Call

**远程调用**

被调用方法的具体实现不在程序运行本地, 而是在别的某个远程地.

例如, 通常调用一个php中的方法 , 像这样的函数localAdd\(10, 20\) , localAdd方法的具体实现要么是用户自己定义的 , 要么是php库函数中自带的 , 也就说在localAdd方法的代码实现在本地 , 它是一个本地调用.

**远程调用原理**

比如A\(client\)调用B\(server\)提供的remoteAdd方法 :

1. 首先A与B之间建立一个TCP
2. 然后A把需要调用的方法名\(这里是remoteAdd\)以及方法参数\(10, 20\)序列化成字节流发送出去

3. B接受A发送过来的字节流, 然后反序列化得到目标方法名,方法参数,接着执行相应的方法调用\(可能是localAdd\)并把结果返回

4. A接受远程调用结果 , 输出结果

RPC框架就是把上面的几点些细节封装起来 , 给用户暴露简单友好的API使用 .

**远程调用的好处**

解耦 , 当server需要对方法内实现修改时 , client完全感知不到 , 不用做任何变更 . 这种方式在跨部门 , 跨公司合作的时候经常用到 .

#### RPC与REST的区别

两者都是client/server模式 .

REST API 和 RPC 都是在`Server端`把一个个函数封装成接口暴露出去 , 以供`Client端`调用 .

* REST API 是基于`HTTP协议`的 , REST致力于通过http协议中的POST/GET/PUT/DELETE等方法和一个可读性强的URL来提供一个http请求 . 
* RPC可以不基于HTTP协议 , 如果是两种后端语言的交互 , 用 RPC 可以获得更好的性能 , 省去了HTTP报头等一系列东西 , 也更容易配置 . 

> 果是前端通过 AJAX 调用后端 , 那么用 REST API 的形式比较好 .

#### RPC与Socket的区别

RPC（远程过程调用）采用客户机/服务器模式实现两个进程之间相互通信 . socket是RPC经常采用的通信手段之一 , RPC是在Socket的基础上实现的 , 它比socket需要更多的网络和系统资源 . 除了Socket , RPC还有其他的通信方法 . 比如 , http , 操作系统 , 自带的管道等技术来实现对于远程程序的调用 .

> 查看下篇内容 , 细说socket .

#### RPC框架 - PHP

PHPRPC\(hprose\) - 轻量级框架

* http://www.phprpc.org/
* https://github.com/hprose/hprose-php

yar - 鸟哥开发的

thrift - facebook的,贡献给apache了 , http://thrift.apache.org/

gRPC - http://www.grpc.io/

swoole - PHP的异步、并行、高性能网络通信引擎 , http://www.swoole.com/

还有阿里巴巴开源的Dubbo和当当网扩展的Dubbox , java的












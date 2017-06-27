# Nginx处理业务逻辑

在经典的LNMP架构中 , 客户端请求到达Nginx后 , Nginx通过查找location命令 , 将所有以"php"为后缀的文件都交给PHP处理 . 

为了弥补Nginx不能处理业务的缺陷 , 可以使用Nginx的Lua模块 . 业界广泛使用的OpenResty项目把Lua语言嵌入Nginx中 , 同时集成了大量实用的模块以方便开发人员使用 . 

OpenResty的开发者章亦春 , 还维护了很多其他的Nginx模块 , 例如 , SRCache等等 . 

https://openresty.org/cn/




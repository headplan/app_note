# 基本的系统优化

#### 开机自启动服务优化

Linux启动时会首先启动一个称为init的进程 , 然后由其来启动后面的任务 , 包括多用户环境 , 网络等 . 运行级是操作系统当前运行的功能级别 , 这个级别从1到6 , 具有不同的功能 , 这些级别在/etc/inittab文件里指定, 这个文件是init进程寻找的主要文件 .

可在/etc/inittab上看到描述 .

* 0 - 表示关机
* 1 - 表示单用户模式
* 2 - 无网络连接的多用户命令行模式
* 3 - 有网络连接的多用户命令行模式
* 4 - 不用
* 5 - 带图形界面的多用户模式
* 6 - 重新启动

可以用runlevel查看当前运行的级别 , 但这个命令只能在root下运行 .

chkconfig命令主要用来更新和查询系统服务的运行级信息 .

语法 :

chkconfig \[--add\] \[--del\] \[--list\] \[系统服务\]

--add : 添加系统服务

--del : 删除系统服务

--list : 显示所有运行级系统服务的运行状态信息

如果需要更新当前系统服务的运行级别时 , 使用下面的命令 :

chkconfig \[--level &lt;等级代号&gt;\] \[系统服务\] \[on/off/reset\]

--level : 服务的等级

--on : 开启系统服务

--off : 关闭系统服务

--reset : 重置系统服务

例如 :

1. 把Nginx的启动脚本放在/etc/init.d/目录下 , 完整的路径为/etc/init.d/nginx
2. 在Nginx加入系统服务  
   chkconfig --add nginx

3. 修改Nginx服务的运行级别  
   chkconfig --level 35 nginx on

#### 增大文件描述符

对Linux内核来说 , 所有打开的文件都通过文件描述符引用 . Linux系统中经常出现的错误 ,

Too many open files 就是由于打开的文件数超过了文件描述符的限制导致 .

查询系统文件描述符大小的命令 :

```
ulimit -n
```

修改方法 :

1. 只对当前Session有效的修改方法
   ```
   ulimit -HSn 65535 
   ulimit -n
   ```
2. 修改配置文件 , 永久生效

   ```
   # 在/etc/security/limits.conf中加入
   * - nofile 65535
   ```

   内核参数对文件描述符也有限制 , 如果设置的值大于内核的限制也是不行的 , 查看内核参数中文件描述符的值 :

   ```
   sysctl -a | grep file-max
   fs.file-max = 1024
   ```

   修改内核参数中文件描述符的值  
   `sysctl -w fs.file-max=65535`




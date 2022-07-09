# Install-Redis-Software

# 1、Download

- 下载安装包！Redis-5.0.8.tar.gz
- 解压Redis的安装包！程序一般放在 `/opt` 目录下

![](D:\my_notebook\【狂神说Java】Redis最新超详细版教程通俗易懂\QQ截图20211117091616.png)

# 2、Installation

- 解压，Redis.tar.gz文件

```shell
tar -zxvvf redis-6.2.6.tar.gz
```

-  然后进入Redis目录下

```shell
cd redis-6.2.6
```

-  注意 Redis C语言开发 安装需要 GCC环境，才能正常运行。所以下载GCC环境，以及执行编译。

```shell
yum install gcc-c++
make
make install
```

- cd 进入`src` 目录

```shell
cd src
```

- 启用Redis.conf中配置，开启Redis服务

```shell
./redis-server ../redis.conf
```



![](D:\my_notebook\【狂神说Java】Redis最新超详细版教程通俗易懂\QQ截图20211117093333.png)

# 3、Start

> 注意：这样开启服务，会有问题，它以前台模式运行服务，这样还需要另外一台连接服务器来对 Redis 操作,并且当下没办法实现远程连接。

修改 `redis.conf` 配置文件 设置为 `daemonize  yes` 支持后台运行！  `bind 0.0.0.0` 绑定任意IP 表示全部的IP都可以连接上去！  `protected-mode no` 关闭安全连接

修改完成后，查看系统进程是否存在Redis服务，杀死Redis服务，并重新开启

```shell
ps -ef |grep redis
kill pid
```


# Install-RabbitMQ-Software

# 1、Download

- 安装Erlang，下载地址：http://erlang.org/download/otp_win64_21.3.exe

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211214150756-16394665835931.jpg)



# 2、Installation

- 安装RabbitMQ，下载地址：https://dl.bintray.com/rabbitmq/all/rabbitmq-server/3.7.14/rabbitmq-server-3.7.14.exe

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211214150831-16394665940243.jpg)

# 3、Start

- 进入RabbitMQ安装目录下的`sbin`目录；切出`cmd`，执行下面命令

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211214152332.jpg)

```shell
rabbitmq-plugins enable rabbitmq_management
```

```properties
网站: http://localhost:15672/
默认账号：guest guest
```







​	

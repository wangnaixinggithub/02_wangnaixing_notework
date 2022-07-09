# 001-Install-MongoDB-Software

# 1、Download

- 下载MongoDB的Docker镜像

```shell
docker pull mongo:4.2.5
```

# 2、Installation

- 使用Docker命令,启动MongoDB服务；要求授权登录。

```shell
docker run -p 27017:27017 --name mongo -v /mydata/mongo/db:/data/db -d mongo:4.2.5 --auth
```

- 进入容器中的MongoDB客户端内部

```
docker exec -it mongo mongo
```

- 之后在`admin`集合中创建一个账号用于连接，这里创建的是基于`root`角色的超级管理员帐号；

```shell
use admin
db.createUser({
    user: 'mongoadmin',
    pwd: 'wangnaixing',
    roles: [ { role: "root", db: "admin" } ] });
```

- 创建完成后验证是否可以登录；

```shell
db.auth("mongoadmin","wangnaixing")
```

![](D:\my_notebook\typora笔记图片统一管理处\QQ截图20211206165523-163946859478315.jpg)

# 3、Start

- 此刻，MongoDB容器已经在Linux系统中运行了，此时系统防火墙放行 MongoDB容器端口。

```shell
firewall-cmd --zone=public --add-port=27017/tcp --permanent
systemctl restart firewalld.service
```

![](D:\my_notebook\typora笔记图片统一管理处\QQ截图20211206170315-16387816815135-163946867054117.jpg)
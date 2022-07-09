# Install-RabbitMQ-Software

# 1、Download

- 下载`rabbitmq 3.7.15`的Docker镜像

```shell
docker pull rabbitmq:3.7.15
```

# 2、Installation

- 使用Docker命令启动服务

```
systemctl restart  docker
docker run -p 5672:5672 -p 15672:15672 --name rabbitmq -d rabbitmq:3.7.15
```

- 进入容器中的RabbitMQ客户端内部

```
docker exec -it rabbitmq /bin/bash
```

- 之后，启动RabbitMQ

```
rabbitmq-plugins enable rabbitmq_management
```

# 3、Start

- 此刻，RabbitMQ容器已经在Linux系统中运行了，此时系统防火墙放行 RabbitMQ容器端口。

```
firewall-cmd --zone=public --add-port=15672/tcp --permanent
firewall-cmd --zone=public --add-port=5672/tcp --permanent
firewall-cmd --reload
```

```
Linux: http://106.55.226.70:15672/
默认账号：guest guest
```


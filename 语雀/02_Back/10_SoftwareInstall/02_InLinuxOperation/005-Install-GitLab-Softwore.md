# Install-GitLab-Softwore

# 1、Download

```shell
$ docker pull gitlab/gitlab-ce
```

# 2、Install

```shell
$ docker run --detach \
  --publish 10443:443 --publish 1080:80 --publish 1022:22 \
  --name gitlab \
  --restart always \
  --volume /mydata/gitlab/config:/etc/gitlab \
  --volume /mydata/gitlab/logs:/var/log/gitlab \
  --volume /mydata/gitlab/data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
```

# 3、Start

```shell
# 开启1080端口
firewall-cmd --zone=public --add-port=1080/tcp --permanent

# 重启防火墙才能生效
systemctl restart firewalld

# 查看已经开放的端口
firewall-cmd --list-ports
```

```
访问地址：http://106.55.226.70:1080/
```

- 可以通过docker命令动态查看容器启动日志来知道`gitlab`是否已经启动完成

```shell
$ docker logs gitlab -f
```

![](D:\my_notebook\typora笔记图片统一管理处\QQ截图20211214143440.jpg)


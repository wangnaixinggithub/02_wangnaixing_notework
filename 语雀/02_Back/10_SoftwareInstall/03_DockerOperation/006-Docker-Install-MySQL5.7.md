# Docker-Install-MySQL5.7

# 1、Download

```shell
docker pull mysql:5.7
```

# 2、Run

```shell
# 重启 Docker
systemctl restart docker
```

```shell
docker run -d -p 3306:3306  -v /home/mysql/config:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql  -e MYSQL_ROOT_PASSWORD=root  --name mysql01  mysql:5.7 
```

```shell
# 1、防火墙开放3306端口
firewall-cmd --zone=public --add-port=3306/tcp --permanent
# 2、重启动防火墙服务
systemctl restart firewalld.service
```


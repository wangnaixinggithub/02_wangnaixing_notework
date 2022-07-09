# Install-Docker

# 1、Downloads And Install

- 安装`yum-utils`；

```shell
yum install -y yum-utils device-mapper-persistent-data lvm2
```

- 为yum源添加docker仓库位置；

```shell
 yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

- 安装docker服务;

```shell
yum install docker-ce
```

# 2、Start

```shell
systemctl start docker
```


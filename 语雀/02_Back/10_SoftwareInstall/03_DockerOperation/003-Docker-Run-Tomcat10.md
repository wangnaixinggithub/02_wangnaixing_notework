# Docker-Run-Tomcat10

# 1、拉取Tomcat10镜像

```shell
$ docker pull tomcat
```

# 2、运行该镜像实例

```shell
$ docker run -d -p 80:8080 --name tomcat01 tomcat
```

# 3、复制webapp.dist中Root目录文件

> 在阿里下的镜像，已经把webapps目录下的东西统统删除了，这些东西在webapps.dist中有。
>
> 而webapps下一个文件夹代表一个项目。如果项目夹名称为ROOT，表示：根工程/，这样就不用加IP:端口/项目名访问了

```shell
docker exec -it tomcat01 /bin/bash

cd webapps.dist

mkdir /usr/local/tomcat/webapps/ROOT

cp -rf ./ROOT/*  /usr/local/tomcat/webapps/ROOT
```

# 4、访问测试

```
http://47.106.187.56:80
```

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211213002315.jpg)
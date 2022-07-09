# 002-Docker-Command

# 1、docker search

```
docker search java
```

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211214101115.jpg)

# 2、`https://hub.docker.com`

- 搜索需要的镜像：比如`nginx`

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211214101402.jpg)

- 查看镜像支持的版本

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211214101456.jpg)

- 进行镜像的下载操作

```
docker pull nginx:1.17.0
```

# 3、docker pull

```
docker pull java:8
```

# 4、docker images

- 列出Docker容器中所有存在的镜像

```
docker images
```

# 5、docker rmi   docker rmi -f 

- 根据名称删除镜像

```
docker rmi java:8
```

- 强制删除镜像

```
docker rmi -f java:8
```

- 强制删除所有镜像

```shell
docker rmi -f $(docker images)
```

# 6、 docker build -t

- 根据`Dockerfile`文件，打包镜像。

```java
# -t 表示指定镜像仓库名称/镜像名称:镜像标签 .表示使用当前目录下的Dockerfile文件
docker build -t mall/mall-admin:1.0-SNAPSHOT .
```

# 7、docker run

- 启动容器

```java
$ docker run -p 80:80 --name nginx \
-e TZ="Asia/Shanghai" \
-v /mydata/nginx/html:/usr/share/nginx/html \
-d nginx:1.17.0
```

- -p：将宿主机和容器端口进行映射，格式为：宿主机端口:容器端口；
- --name：指定容器名称，之后可以通过容器名称来操作容器；
- -e：设置容器的环境变量，这里设置的是时区；
- -v：将宿主机上的文件挂载到宿主机上，格式为：宿主机文件目录:容器文件目录；
- -d：表示容器以后台方式运行。

# 8、docker ps docker ps -a

- 列出运行中的容器：

```shell
docker ps
```

- 列出所有容器：

```
 docker ps -a
```

# 9、docker stop

- 停止一个已经在运行的容器，可以通过`docker start` 再次启动。

```shell
docker stop $ContainerName(or $ContainerId)
```

```shell
docker stop nginx
#或者
docker stop c5f5d5125587
```

# 10、docker kill

- 强制停止容器，杀死后，不能再启动。

```shell
docker kill $ContainerName
```

# 11、docker start

```
docker start $ContainerName
```

# 12、Enter Container Inner

- 进入容器内部，以进入Nginx为例子说明。

```shell
# 先查询出容器的pid
docker inspect --format "{{.State.Pid}}" $ContainerName

# 根据容器的pid进入容器
nsenter --target "$pid" --mount --uts --ipc --net --pid
```

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211214105734.jpg)1

# 13、Enter Container Work Dir

- 进入容器工作目录。

```shell
docker exec -it $ContainerName /bin/bash

# Or 使用root账号进入容器内部
$ docker exec -it --user root $ContainerName /bin/bash
```

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211214112533.jpg)



# 13、GET Container IPAddress

```shell
docker inspect --format '{{ .NetworkSettings.IPAddress }}' $ContainerName
```

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211214111036.jpg)

# 14、Update Container  Start Way

```shell
# 将容器启动方式改为always
docker container update --restart=always $ContainerName
```



# 15、docker rm  docker rm -f

- 删除指定容器。删除容器前，此容器必须停止运行状态。

```
 docker rm $ContainerName
```

- 如果想强制删除容器，哪怕当前为运行状态。可以使用 `docker rm -f`

```
 docker rm -f $ContainerName
```

- 按名称通配符删除容器，比如删除以名称`wangnaixing-`开头的容器：假如说你的容器在运行，你不想stop容器，可以使用强制删除 `rm -f`

```
docker rm `docker ps -a | grep wangnaixing-* | awk '{print $1}'`
```

- 强制删除所有容器

```
docker rm -f $(docker ps -a -q)
```

# 16、docker logs

- 查看容器产生的全部日志

```
docker logs $ContainerName
```

- 动态查看容器产生的日志：

```shell
docker logs -f $ContainerName
```

# 17、docker stats

- 查看指定容器资源占用状况，比如`cpu`、内存、网络、IO状态：

```
docker stats $ContainerName -a
```

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211214111424.jpg)

# 18、docker system df

- 查看容器磁盘使用情况

```
docker system df
```

![](https://wnxbucket-001.oss-cn-guangzhou.aliyuncs.com/myblog/QQ%E6%88%AA%E5%9B%BE20211214111629.jpg)

# 19、docker network ls

- 查看全部网络

```
docker network ls
```

```shell
[root@VM-20-3-centos ~]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
600e1b981b6f   bridge    bridge    local
bb14c3db536f   host      host      local
de995361d185   none      null      local
[root@VM-20-3-centos ~]# 
```

# 20、 docker network create

```
docker network create -d bridge my-bridge-network
```

```shell
[root@VM-20-3-centos ~]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
600e1b981b6f   bridge    bridge    local
bb14c3db536f   host      host      local
de995361d185   none      null      local
[root@VM-20-3-centos ~]# docker network create -d bridge my-bridge-network
a7b72660f9b6d300ee5b66e5ef516d8796b93df6888284d1ccf6e53217686d2b
[root@VM-20-3-centos ~]# docker network ls
NETWORK ID     NAME                DRIVER    SCOPE
600e1b981b6f   bridge              bridge    local
bb14c3db536f   host                host      local
a7b72660f9b6   my-bridge-network   bridge    local
de995361d185   none                null      local
[root@VM-20-3-centos ~]# 
```

- 指定容器网络

```
$ docker run -p 80:80 --name nginx \
--network my-bridge-network \
-d nginx:1.17.0
```


# Deploy-SpringBoot-Project-Base-DockerFile

# 1、Need Config

```yaml
# application.yaml db为数据库网络IP
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://db:3306/mall?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: root
    password: root

mybatis:
  mapper-locations:
    - classpath:mapper/*.xml
    - classpath*:com/**/mapper/*.xml

```

```dockerfile
# DockerFile Include Context

# 01.该镜像需要依赖的基础镜像
FROM java:8

# 02.将当前目录下的jar包复制到docker容器的/目录下
ADD mall-tiny-docker-file-0.0.1-SNAPSHOT.jar /mall-tiny-docker-file.jar

# 03.运行过程中创建一个mall-tiny-docker-file.jar文件
RUN bash -c 'touch /mall-tiny-docker-file.jar'

# 04.声明服务运行在8080端口
EXPOSE 8080

# 05.指定docker容器启动时运行jar包
ENTRYPOINT ["java", "-jar","/mall-tiny-docker-file.jar"]

# 06.指定维护者的名字
MAINTAINER wangnaixing
```

# 2、Upload Jar And DockerFile

- 打包SpringBoot项目得到jar
- 把jar和DockerFile一起上传的Linux中。



# 3、MySQL Docker Start

- 构造Jar包成Docker镜像

```shell
docker build -t mall-tiny/mall-tiny-docker-file:0.0.1-SNAPSHOT .
```

- 启动MySQL服务

```shell
docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root  \
-d mysql:5.7
docker exec -it mysql /bin/bash
```

- 设置MySQL远程连接和导入SQL

```shell
mysql -uroot -proot

grant all privileges on *.* to 'root'@'%';

create database mall character set utf8;


```

- window平台，连接MySQL导入SQL.

# 4、SpringBoot Docker Start

```
docker run -p 8080:8080 --name mall-tiny-docker-file \
--link mysql:db \
-v /etc/localtime:/etc/localtime \
-v /mydata/app/mall-tiny-docker-file/logs:/var/logs \
-d mall-tiny/mall-tiny-docker-file:0.0.1-SNAPSHOT
```

# 4、Validation Test

```shell
http://106.55.226.70:8080/swagger-ui.html
```


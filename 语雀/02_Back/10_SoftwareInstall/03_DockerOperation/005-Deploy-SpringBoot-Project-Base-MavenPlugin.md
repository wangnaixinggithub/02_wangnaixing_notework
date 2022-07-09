# Deploy-SpringBoot-Project-Base-MavenPlugin

> 本文主要介绍如何使用Maven插件将SpringBoot应用打包为Docker镜像，并上传到私有镜像仓库Docker Registry的过程。

# 1、Need Config

```xml
<!--pom.xml -->
    <groupId>com.wnx.mall.tiny</groupId>
    <artifactId>mall-tiny-docker</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>mall-tiny-docker</name>
    <description>Demo project for Spring Boot</description>



    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>com.spotify</groupId>
                <artifactId>docker-maven-plugin</artifactId>
                <version>1.1.0</version>
                <executions>
                    <execution>
                        <id>build-image</id>
                        <phase>package</phase>
                        <goals>
                            <goal>build</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <imageName>mall-tiny/${project.artifactId}:${project.version}</imageName>
                    <dockerHost>http://106.55.226.70:2375</dockerHost>
                    <baseImage>java:8</baseImage>
                    <entryPoint>["java", "-jar","/${project.build.finalName}.jar"]
                    </entryPoint>
                    <resources>
                        <resource>
                            <targetPath>/</targetPath>
                            <directory>${project.build.directory}</directory>
                            <include>${project.build.finalName}.jar</include>
                        </resource>
                    </resources>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

```yaml
#application.yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/db?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: root
    password: root

mybatis:
  mapper-locations:
    - classpath:mapper/*.xml
    - classpath*:com/**/mapper/*.xml

```

# 2、Docker Registry 2.0 Start

- 启动 Docker Registry2

```
docker run -d -p 5000:5000 --restart=always --name registry2 registry:2
```

- 还需要修改docker.service配置文件。修改内容见第二行。

```shell
vi /usr/lib/systemd/system/docker.service
```

```
ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock
```

- 支持HTTP上传

```
echo '{ "insecure-registries":["106.55.226.70:5000"] }'>/etc/docker/daemon.json
```

- 重新启动Docker，让新配置生效。

```shell
systemctl daemon-reload
systemctl stop docker
systemctl start docker
```

# 3、MySQL Docker Start

- 启动MySQl容器并进入容器

```shell
docker run -p 3306:3306 --name mysql \
-v /mydata/mysql/log:/var/log/mysql \
-v /mydata/mysql/data:/var/lib/mysql \
-v /mydata/mysql/conf:/etc/mysql \
-e MYSQL_ROOT_PASSWORD=root  \
-d mysql:5.7
docker exec -it mysql /bin/bash
```

- 让远程连接生效

```sql
mysql -uroot -proot

grant all privileges on *.* to 'root'@'%';

create database mall character set utf8;
```

- window远程连接导入，工程SQL脚本。

# 4、SpringBoot Docker Start

- 执行会发现，他会先把你的SpringBoot项目打包，再整成镜像上传到你的`DockerImages`中。

```
Lifecycle> package
```

- 运行SpringBoot 工程。

```shell
docker run -p 8080:8080 --name mall-tiny-docker \
--link mysql:db \
-v /etc/localtime:/etc/localtime \
-v /mydata/app/mall-tiny-docker/logs:/var/logs \
-d mall-tiny/mall-tiny-docker:0.0.1-SNAPSHOT
```

# 4、Validation Test

```javascript
http://106.55.226.70:8080/swagger-ui.html
```


# Deploy-SpringBoot-Local-Environment

> 使用Linux服务器本地环境运行SpringBoot项目



# 1、Clean And Package Project ,Then Upload .jar

上传打包好的后端项目，比如我这边上传到 `/mydata` 目录下。

![image-20220708223422880](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708223422880.png)

# 2、Use Cmd Java -jar

```shell
# 1、验证此虚拟机已经安装了 JDK环境
java -version

# 2、验证此虚拟机已经安装了 Maven环境
mvn -version

#3、后台运行SpringBoot工程 然后回车。
nohup java -jar back-0.0.1-SNAPSHOT.jar &
 
#4、Linux放行8080端口
firewall-cmd --zone=public --add-port=8080/tcp --permanent
 
#5、重启防火墙
firewall-cmd --reload
```

# 3、Valition Suceess

```properties
验证服务跑起来了：http://192.168.206.137:8080/
```


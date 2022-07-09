# Install-Maven-Software

# 1、Download

```
官网：https://maven.apache.org/download.cgi
```

![image-20220708215656443](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708215656443.png)



# 2、Installation

```shell
# 1、进入到/usr目录下
cd /usr

# 2、创建maven文件夹
mkdir maven

# 3、进入到/usr/maven
cd /usr/maven

#4、通过Xftp 从window平台上传.zip 文件到/usr/mavin

#5、通过yum 安装zip解压工具
yum -y install bzip2

#4、对.zip文件进行解压操作
unzip apache-maven-3.8.6-bin.zip

#5、修改系统环境变量
vim /etc/profile

#6、安装Maven相关命令
yum install maven -y

```



```shell
export JAVA_HOME=/usr/java/jdk1.8.0_333
export CLASSPATH=$JAVA_HOME/lib/
export MAVEN_HOME=/usr/maven/apache-maven-3.8.6
export PATH=$PATH:$JAVA_HOME/bin:$MAVEN_HOME/bin
export PATH JAVA_HOME CLASSPATH
```



# 3、Start

```shell
mvn -version
```

![image-20220708221419044](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708221419044.png)


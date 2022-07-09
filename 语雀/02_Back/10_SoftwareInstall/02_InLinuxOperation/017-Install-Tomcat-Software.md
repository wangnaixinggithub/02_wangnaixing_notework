# Install-Tomcat-Software

# 1、Download

```properties
官网: https://tomcat.apache.org/
```

![image-20220708232132739](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708232132739.png)

# 2、Installation

```shell
# 1、进入到/usr 目录下，创建 tomcat 文件夹，并进入到tomcat文件夹
cd /usr
mkdir tomcat
cd /usr/tomcat

# 2、使用ftp，把在window下载好的Tomcat.tar.gz上传到Linux中 /usr/tomcat目录下。

# 3、对Tomcat.tar.gz 进行解压操作
tar -zxvf  apache-tomcat-10.0.22.tar.gz
```

# 3、Start

```shell
# 4、进入解压的bin目录
cd /usr/tomcat/apache-tomcat-10.0.22/bin

# 5、启用执行命令
./startup.sh

# 6、防火墙放行 80 端口

#7、重启防火墙

#8、此时可以通过IP访问
http://192.168.206.137:8080/
```

![image-20220708233924930](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708233924930.png)


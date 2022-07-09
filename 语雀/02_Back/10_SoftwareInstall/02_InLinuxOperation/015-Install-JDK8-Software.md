# Install-JDK8-Software

# 1、Download

```properties
官网：https://www.oracle.com/java/technologies/downloads/#java17
```

![image-20220708194424217](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708194424217.png)



# 2、Installation

```properties
# 1、进入到/usr 目录下，创建java文件夹,并进入到java文件夹。
cd /usr
mkdir java
cd /usr/java

#2、使用ftp，把在window下载好的JDK上传到Linux中 /usr/java目录下。

#3、对JDK.tar.gz包进行解压操作
tar -zxvf jdk-8u333-linux-x64.tar.gz
# 4、修改Linux系统配置，在文件末尾添加以下内容。
vim /etc/profile

```

```properties
export JAVA_HOME=/usr/java/jdk1.8.0_333
export CLASSPATH=$JAVA_HOME/lib/
export PATH=$PATH:$JAVA_HOME/bin
export PATH JAVA_HOME CLASSPATH
```

# 3、Start

```
java -version
```



![image-20220708215047500](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708215047500.png)
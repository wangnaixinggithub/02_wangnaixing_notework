# Install-Tomcat-Software

# 1、Download

官网下载并在无中文目录进行解压即可。

# 2、Install

- 新创建，系统环境变量

```properties
CATALINA_HOME D:\development_environment\apache-tomcat-9.0.56
```

- path变量加入如下引用。

```properties
%CATALINA_HOME%\lib
%CATALINA_HOME%\bin
```

# 3、Config

## 1、LogConfig

> `logging.conf`中 注释掉控制台处理器的编码为`UTF-8`,改成`GBK`.
>
> 原因是：因为window系统本身是`GBK`编解码，Tomcat必须也用`GBK` 才能让控制台输出不乱码。

```properties
java.util.logging.ConsoleHandler.level = FINE
java.util.logging.ConsoleHandler.formatter = org.apache.juli.OneLineFormatter
#java.util.logging.ConsoleHandler.encoding = UTF-8
java.util.logging.ConsoleHandler.encoding = GBK
```

## 2、解决请求查询串乱码

> `server.xml`中 Connector连接器，指定` URIEncoding="UTF-8" `

```xml
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" URIEncoding="UTF-8" />
```

# 4、Start

bin目录，点击startup启动tomcat

```
访问网站：http://localhost:8080
```

![image-20211231114830241](https://s2.loli.net/2021/12/31/6KbdMuSsNApw9ze.png)
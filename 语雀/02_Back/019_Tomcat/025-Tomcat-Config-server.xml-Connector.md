# Tomcat-Config-server.xml-Connector

Connector 用于创建链接器实例。默认情况下，`server.xml` 配置了两个链接器

一个支 持HTTP协议，一个支持AJP协议。

因此大多数情况下，我们并不需要新增链接器配置， 只是根据需要对已有链接器进行优化。

# 1、Default Case

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Connector port="8080" protocol="HTTP/1.1" connectionTimeout="20000"redirectPort="8443" />

<Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
```

# 2、Configuration Instructions

1）` port`： 端口号，Connector 用于创建服务端Socket 并进行监听， 以等待客户端请求 链接。如果该属性设置为0，Tomcat将会随机选择一个可用的端口号给当前Connector 使用。

2）` protocol `： 当前Connector 支持的访问协议。 默认为 HTTP/1.1 ， 并采用自动切换 机制选择一个基于 JAVA NIO 的链接器或者基于本地APR的链接器（根据本地是否含有 Tomcat的本地库判定）。

如果不希望采用上述自动切换的机制， 而是明确指定协议， 可以使用以下值。

```properties
Http协议：
org.apache.coyote.http11.Http11NioProtocol ， 非阻塞式 Java NIO 链接器
org.apache.coyote.http11.Http11Nio2Protocol ， 非阻塞式 JAVA NIO2 链接器
org.apache.coyote.http11.Http11AprProtocol ， APR 链接器

AJP协议 ：
org.apache.coyote.ajp.AjpNioProtocol ， 非阻塞式 Java NIO 链接器
org.apache.coyote.ajp.AjpNio2Protocol ，非阻塞式 JAVA NIO2 链接器
org.apache.coyote.ajp.AjpAprProtocol ， APR 链接器
```

3） `connectionTimeOut` : Connector 接收链接后的等待超时时间， 单位为 毫秒。 -1 表 示不超时。

4）` redirectPort`：当前Connector 不支持SSL请求， 接收到了一个请求， 并且也符合 security-constraint 约束， 需要SSL传输，Catalina自动将请求重定向到指定的端口。

5）` executor` ： 指定共享线程池的名称， 也可以通过`maxThreads`、`minSpareThreads` 等属性配置内部线程池。

6）` URIEncoding` : 用于指定编码URI的字符编码， Tomcat8.x版本默认的编码为` UTF-8` , Tomcat7.x版本默认为`ISO-8859-1`。

# 3、Whole Config



完整的配置如下：

```xml
<Connector port="8080"
    protocol="HTTP/1.1"
    executor="tomcatThreadPool"
    maxThreads="1000"
    minSpareThreads="100"
    acceptCount="1000"
    maxConnections="1000"
    connectionTimeout="20000"
    compression="on"
    compressionMinSize="2048"
    disableUploadTimeout="true"
    redirectPort="8443"
URIEncoding="UTF‐8" />
```


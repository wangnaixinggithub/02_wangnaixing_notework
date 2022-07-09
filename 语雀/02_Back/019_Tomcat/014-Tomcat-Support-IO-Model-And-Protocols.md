# Tomcat-Support-IO-Model-And-Protocols

在Coyote中 ， Tomcat支持的多种I/O模型和应用层协议，具体包含哪些IO模型和应用层 协议，请看下表：

# 1、Support IO Model

Tomcat 支持的IO模型（自8.5/9.0 版本起，Tomcat 移除了 对 BIO 的支持）：

![image-20220705224126081](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705224126081.png)

# 2、 Support Application Protocols

Tomcat 支持的应用层协议 ：

![image-20220705224213484](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705224213484.png)

# 3、Layer Divide

协议分层 ：

![image-20220705224321621](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705224321621.png)

在 8.0 之前 ， Tomcat 默认采用的I/O方式为 BIO ， 之后改为 NIO。 无论 NIO、NIO2 还是 APR， 在性能方面均优于以往的BIO。


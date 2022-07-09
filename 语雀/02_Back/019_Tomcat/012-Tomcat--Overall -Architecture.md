# Tomcat--Overall -Architecture

Tomcat要实现两个核心功能：

- 1） 处理Socket连接，负责网络字节流与Request和Response对象的转化。

- 2） 加载和管理Servlet，以及具体处理Request请求。



因此Tomcat设计了两个核心组件**连接器**和**容器**来分别做这 两件事情。

- 1）连接器负责对外交流，Connector

- 2）容器负责内部处理。    Container

![image-20220705223112122](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705223112122.png)
# Tomcat-Contater- Catalina

Catalina 是Tomcat Servlet 容器实现

# 1、Architecture Diagram

![image-20220705225732289](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705225732289.png)

Catalina负责管理Server，而Server表示着整个服务器。Server下面有多个 服务Service，每个服务都包含着多个连接器组件Connector（Coyote 实现）和一个容器 组件Container。在Tomcat 启动的时候， 会初始化一个Catalina的实例。

# 2、Component Responsibilities

Catalina 各个组件的职责：

.![image-20220705230042709](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705230042709.png)
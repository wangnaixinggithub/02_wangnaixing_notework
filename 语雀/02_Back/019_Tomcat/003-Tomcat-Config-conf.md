# Tomcat-Config-conf

- conf:用于存放Tomcat的相关配置文件

![image-20220705191410835](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705191410835.png)

```properties
Catalina:用于存储针对每个虚拟机的Context配置

context.xml:用于定义所有web应用均需加载的Context配置，如果web应用指定了自己的context.xml，该文件将被覆盖

catalina.properties:Tomcat 的环境变量配置

catalina.policy:Tomcat 运行的安全策略配置

logging.properties:Tomcat 的日志配置文件， 可以通过该文件修改Tomcat 的日志级别及日志路径等

server.xml:Tomcat 服务器的核心配置文件

tomcat-users.xml:定义Tomcat默认的用户及角色映射信息配置

web.xml:Tomcat 中所有应用默认的部署描述文件，主要定义了基础Servlet和MIME映射。
```


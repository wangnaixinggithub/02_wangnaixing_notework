# Tomcat-Module-layered-Structure-Diagram

![image-20220705234125588](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705234125588.png)

Tomcat 本质上就是一款 Servlet 容器， 因此Catalina 才是 Tomcat 的核心 ， 其他模块 都是为Catalina 提供支撑的。 比如 ： 通过Coyote 模块提供链接通信，Jasper 模块提供 JSP引擎，Naming 提供JNDI 服务，Juli 提供日志服务。
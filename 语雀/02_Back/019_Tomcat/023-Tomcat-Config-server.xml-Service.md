# Tomcat-Config-server.xml-Service

该元素用于创建 Service 实例，默认使用 org.apache.catalina.core.StandardService。

默认情况下，Tomcat 仅指定了Service 的名称， 值为 "Catalina"。

Service 可以内嵌的 元素为 ： Listener、Executor、Connector、Engine

- Listener 用于为Service 添加生命周期监听器

- Executor 用于配置Service 共享线程池

- Connector 用于配置 Service 包含的链接器

- Engine 用于配置Service中链接器对应的Servlet 容器引擎

```xml
<Service name="Catalina">
...
</Service>
```

一个Server服务器，可以包含多个Service服务。




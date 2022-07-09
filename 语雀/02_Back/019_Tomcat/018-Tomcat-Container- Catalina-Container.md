# Tomcat-Container- Catalina-Container

Container含有4种容器，分别是Engine、Host、Context和Wrapper。这4种容器不是平行关系，而是父子关系。 

Tomcat通过一种分层的架构，使得Servlet容器具有很好的灵活性。

![image-20220705233042351](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705233042351.png)

各个组件的含义 ：

![image-20220705233213999](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705233213999.png)

我们也可以再通过Tomcat的server.xml配置文件来加深对Tomcat容器的理解。Tomcat 采用了组件化的设计，它的构成组件都是可配置的，其中最外层的是Server，其他组件 按照一定的格式要求配置在这个顶层容器中。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Server port="8005" shutdown="SHUTDOWN">
  <Service name="Catalina">
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
    <Engine name="Catalina" defaultHost="localhost">
      <Host name="localhost"  appBase="webapps" unpackWARs="true" autoDeploy="true">
      </Host>
    </Engine>
  </Service>
</Server>
```

那么，Tomcat是怎么管理这些容器的呢？你会发现这些容器具有父子关系，形成一个树 形结构，你可能马上就想到了设计模式中的组合模式。

没错，Tomcat就是用组合模式来 管理这些容器的。

具体实现方法是，所有容器组件都实现了Container接口，因此组合模式可以使得用户对单容器对象和组合容器对象的使用具有一致性。这里单容器对象指的 是最底层的Wrapper，组合容器对象指的是上面的Context、Host或者Engine。



![image-20220705233736888](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705233736888.png)
# Tomcat-Why-Use-Servlet-Servlet-Container

浏览器发给服务端的是一个HTTP格式的请求，HTTP服务器收到这个请求后，需要调用服务端程序来处理。

所谓的服务端程序就是你写的Java类，一般来说不同的请求需要由不同 的Java类来处理。

# 1、为了解构！

![image-20220705222420285](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705222420285.png)



- 1） 图1 ， 表示HTTP服务器直接调用具体业务类，它们是紧耦合的。
-  2） 图2，HTTP服务器不直接调用业务类，而是把请求交给容器来处理，容器通过 Servlet接口调用业务类。



因此Servlet接口和Servlet容器的出现，达到了HTTP服务器与 业务类解耦的目的。而Servlet接口和Servlet容器这一整套规范叫作Servlet规范。 

Tomcat按照Servlet规范的要求实现了Servlet容器，同时它们也具有HTTP服务器的功 能。

作为Java程序员，如果我们要实现新的业务功能，只需要实现一个Servlet，并把它注册到Tomcat（Servlet容器）中，剩下的事情就由Tomcat帮我们处理了。




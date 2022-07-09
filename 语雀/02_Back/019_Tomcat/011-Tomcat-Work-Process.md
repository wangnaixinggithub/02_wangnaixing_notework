# Tomcat-Work-Process



![image-20220705222639016](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705222639016.png)

当客户请求某个资源时，HTTP服务器会用一个`ServletRequest`对象把客户的请求信息封 装起来，然后调用Servlet容器的service方法，Servlet容器拿到请求后，根据请求的URL 和Servlet的映射关系，找到相应的Servlet



如果Servlet还没有被加载，就用反射机制创 建这个Servlet，并调用Servlet的`init`方法来完成初始化，接着调用Servlet的service方法 来处理请求



Servlet容器把`ServletResponse`对象返回给HTTP服务器，HTTP服务器会把响应发送给 客户端。




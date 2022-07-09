# Tomcat-Connector-Coyote

# 1、是什么？

Coyote 是Tomcat的连接器框架的名称,Coyote 是Tomcat的连接器框架的名称。

客户端通过Coyote与服务器建立连接、发送请求并接受响应 。

# 2、做什么？

Coyote 封装了底层的网络通信（Socket 请求及响应处理），为Catalina 容器提供了统一 的接口，使Catalina 容器与具体的请求协议及IO操作方式完全解耦。

# 3、怎么做？

Coyote 将Socket 输 入转换封装为 Request 对象，交由Catalina 容器进行处理，处理请求完成后, Catalina 通 过Coyote 提供的Response 对象将结果写入输出流 。



Coyote 作为独立的模块，只负责具体协议和IO的相关操作， 与Servlet 规范实现没有直 接关系，因此即便是 Request 和 Response 对象也并未实现Servlet规范对应的接口， 而 是在Catalina 中将他们进一步封装为`ServletRequest` 和` ServletResponse` 。



![image-20220705223801831](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220705223801831.png)




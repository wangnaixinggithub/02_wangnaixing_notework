# Tomcat-Connector-Coyote-Inner-Component

连接器组件:

![image-20220705231343803](015-Tomcat-Connector-Coyote-InnerCompotent.assets/image-20220705231343803.png)

# 1、EndPoint

- 1） EndPoint ： Coyote 通信端点，即通信监听的接口，是具体Socket接收和发送处理器，是对传输层的抽象，因此EndPoint用来实现TCP/IP协议的。

- 2） Tomcat 并没有EndPoint 接口，而是提供了一个抽象类`AbstractEndpoint` ， 里面定 义了两个内部类：Acceptor和`SocketProcessor`。Acceptor用于监听Socket连接请求。 

  `SocketProcessor`用于处理接收到的Socket请求，它实现Runnable接口，在Run方法里 调用协议处理组件Processor进行处理。

​	 为了提高处理能力，`SocketProcessor`被提交到 线程池来执行。而这个线程池叫作执行器（Executor)，

# 2、Processor

Processor ： Coyote 协议处理接口 ，如果说EndPoint是用来实现TCP/IP协议的那么 Processor用来实现HTTP协议，

Processor接收来自EndPoint的Socket，读取字节流解析成Tomcat Request和Response对象，并通过Adapter将其提交到容器处理， 

Processor是对应用层协议的抽象。

# 3、ProtocolHandler

Coyote 协议接口， 通过Endpoint 和 Processor ， 实现针对具体协 议的处理能力。

Tomcat 按照协议和I/O 提供了6个实现类 ： `AjpNioProtocol` ， `AjpAprProtocol`， `AjpNio2Protocol` ，` Http11NioProtocol` ，`Http11Nio2Protocol` ，` Http11AprProtocol`。

我们在配置tomcat/conf/server.xml 时 ， 至少要指定具体的 ProtocolHandler , 当然也可以指定协议名称 



# 4、Adapter

由于协议不同，客户端发过来的请求信息也不尽相同，Tomcat定义了自己的Request类 来“存放”这些请求信息。

ProtocolHandler接口负责解析请求并生成Tomcat Request类。 

但是这个Request对象不是标准的`ServletRequest`，也就意味着，不能用Tomcat Request作为参数来调用容器。

Tomcat设计者的解决方案是引入`CoyoteAdapter`，这是 适配器模式的经典运用，连接器调用`CoyoteAdapter`的`#sevice()`方法，传入的是Tomcat Request对象，`CoyoteAdapter`负责将`Tomcat Request`转成`ServletRequest`，再调用容器的`#service()`方法。
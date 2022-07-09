# NIO-Selector-HelloWorld

选择器（Selector）是`SeIectabIeChannIe`对象的多路复用器.

Selector可以同时监控多个 `SelectableChannel` 的IO状况，也就是说，利用Selector可使一个单独的线程管理多个Channel。

 Selector是非阻塞IO的核心

![image-20220701235913206](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220701235913206.png)

java的NIO，用非阻塞的IO方式。可以用一个线程，处理多个的客户端连接，就会使用到Selector （选择器）



这样就可以只用一个单线程去管理多个通道，也就是管理多个连接和请求。 只有在连接／通道真正有读写事件发生时，才会进行读写，就大大地减少了系统开销，并且不必为 每个连接都 创建一个线程，不用去维护多个线程 避免了多线程之间的上下文切换导致的开销.
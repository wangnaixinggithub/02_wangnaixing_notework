# NIO-Channel-HelloWorld



通道（Channel)：由`java.nio.channels`包定义的。

Channel表示IO源与目标打开的连接。Channel类似 于传统的“流”。只不过Channel本身不能直接访问数据，Channel只能与Buffer进行交互。 

- 1、`NIO`的通道类似于流，但有些区别如下 通道可以同时进行读写，而流只能读或者只能写 通道可以实现异步读写数据 通道可以从缓冲读数据，也可以写数据到缓冲

- 2、`BlO`中的stream是单向的，例如`FilelnputStream`对象只能进行读取数据的操作，而NIO中的`通道 （Channel) ` 是双向的，可以读操作，也可以写操作。

- 3、`Channel`在`NIO`中是一个接口



```java
public interface Channel extends Closeable {
	public boolean isOpen();
	public void close() throws IOException;
}
```

![image-20220701200545797](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220701200545797.png)
# AIO-HelloWorld

Java AIO(NIO.2)：异步非阻塞，服务器实现模式为一个有效请求一个线程，客户端的I/O请求都是 由OS先完成了再通知服务器应用去启动线程进行处理。

 AIO是异步非阻塞，基于NIO，可以称之为NIO2.0



![image-20220702084423798](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220702084423798.png)



与NIO不同，当进行读写操作时，只须直接调用API的read或write方法即可，这两种方法均为异步的

对于读操作而言，当有流可读时，操作系统会将可读的流传入read方法的缓冲区，

对于写操作而言，当 操作系统将 write方法传递的流写入完毕时，操作系统主动通知应用程序 。 

即可以理解为，read/write方法都是异步的，完成后会主动调用回调函数。

在JDK1.7中，这部分内容被 称作 NIO.2，主要在java.nio.channel包下增加了下面四个异步通道：

 AsynchronousSocketChannel

 AsynchronousServerSocketChannel 

AsynchronousFileChannel

 AsynchronousDatagramChanne
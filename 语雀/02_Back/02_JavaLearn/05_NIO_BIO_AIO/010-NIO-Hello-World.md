# NIO-Hello-World

# 1、What is NIO

- Java NlO (New lO）也有人称之为java non-blocking IO  是Java 1.4版本开始引入的一个新的IO API，可以 替代标准的Java lO API。 
- NIO与原来的IO有同样的作用和目的，但是使用的方式完全不同，NIO支持面向缓冲区的、基于通道的IO操作。
- NIO将以更加高效的方式进行文件的读写操作。
- NIO可以理解为非阻塞IO，传统的IO 的read和write只能阻塞执行，线程在读写期间不能干其他事情。比如调用socket. read(）时，如果服务器一 直没有数据传输过来，线程就一直阻塞，而NIO中 可以配置socket为非阻塞模式。

- NIO相关类都被放在java.nio包及子包下，并且对原Java.io包中的很多类进行改写。
- NIO有三大核心部分：Channel（通道）,Buffer(缓冲区）,Selector（选择器）

# 2、Working Process

![image-20220701093138400](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220701093138400.png)

- 每个channel都会对应一个Buffer
- 一个线程对应Selector，一个Selector对应多个channel（连接）
- 程序切换到哪个channel是由事件决定的
- Selector会根据不同的事件，在各个通道上切换
- Buffer就是一个内存块，底层是一个数组
- 数据的读取写入是通过Buffer完成的，BIO中要么是输入流，或者是输出流，不能双向，但是NIO 的Buffer是可以读也可以写。
- Java NIO系统的核心在于：通道（Channel）和缓存区（Buffer)。通道表示打开到IO设备（例如： 文件、套接字） 的连接。若需要使用NlO系统，需要获取用于连接IO设备的通道以及用于容纳数据 的缓冲区。然后操作缓 冲区，对数据进行处理。简而言之，Channel负责传输，Buffer负责存取数 据
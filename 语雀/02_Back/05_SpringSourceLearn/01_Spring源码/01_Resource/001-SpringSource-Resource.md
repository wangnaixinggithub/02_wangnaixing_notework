# SpringSource-Resource

> 在Spring的世界,所有的资源抽象为Resouce接口类。
>
> 这些资源可能来自操作系统的文件系统，可能来自网络URL，可能来自项目的类路径。也可能是一个网络字节序列流，字节数组，字节输入流。
>
> Spring把他们都看为是Resource资源。

# 1、Resource

```java
public interface Resource extends InputStreamSource {}
```

# 2、InputStreamSource

```java
public interface InputStreamSource {
	//规定了Resource接口的实现类，必须提供获取当前资源的输入流的接口方法。
	InputStream getInputStream() throws IOException;
    //如果我们通过某些手段得到了Resourse类对象，我们就可以通过getInputStream()的方法得到资源的流。
}
```

# 3、Interface Method

```java
//定义了他作为Resource应当具备的特性

public interface Resource extends InputStreamSource {
    	//1、判断性方法
   		boolean exists();
        default boolean isReadable() {return true;}
        default boolean isOpen() {return false;}
        default boolean isFile() {return false;}
    
    	//2、获取实际资源
        URL getURL() throws IOException;
        URI getURI() throws IOException
        File getFile() throws IOException;
        default ReadableByteChannel readableChannel() throws IOException {
            return Channels.newChannel(getInputStream());
        }
    
    	//3、资源的一些基本属性
        long contentLength() throws IOException;
        long lastModified() throws IOException;
        Resource createRelative(String relativePath) throws IOException;
        @Nullable
        String getFilename();
        String getDescription();
    
}
```


# NIO-Buffer-#capacity()

- 获取到缓冲区的容量大小

```java
package com.wnx.model;

import java.nio.*;

public class BufferTest {
    public static void main(String[] args) {
        ByteBuffer buffer = ByteBuffer.allocate(10);
        //1。获取Buffer容量
        int capacity = buffer.capacity();
        System.out.println(capacity);   //10

    }
}

```


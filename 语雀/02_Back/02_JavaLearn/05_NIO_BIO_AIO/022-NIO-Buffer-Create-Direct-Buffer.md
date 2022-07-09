# BIO-Buffer-Create-Direct-Buffer

- 创建一个直接内存的缓冲区。

```java
package com.wnx.model;

import java.nio.*;
import java.util.Arrays;

public class BufferTest {
    public static void main(String[] args) {

        //创建一个直接内存的缓冲区
        ByteBuffer buffer = ByteBuffer.allocateDirect(20);

        System.out.println(buffer.isDirect());     //true

    }
}
```

使用场景 

-  有很大的数据需要存储，他的生命周期又很长 

- 适合频繁的IO操作，比如网络并发场景
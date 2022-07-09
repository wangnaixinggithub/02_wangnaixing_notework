# BIO-Buffer-#get()

- 获取缓存区到数据到字节数组中。

```java
package com.wnx.model;

import java.nio.*;
import java.util.Arrays;

public class BufferTest {
    public static void main(String[] args) {
        
        ByteBuffer buf = ByteBuffer.allocate(12);
        buf.put("wangnaixing".getBytes());

        
        //1.读取数据 到字节数组。
        buf.flip();
        byte[] b = new byte[4];
        buf.get(b);
        
        //2.输出结果
        System.out.println( new String(b));//wang

    }
}
```


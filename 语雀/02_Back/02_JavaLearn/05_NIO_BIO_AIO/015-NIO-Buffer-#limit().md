# NIO-Buffer-#limit()

- 设置缓冲区的限制量

```java
package com.wnx.model;

import java.nio.*;

public class BufferTest {
    public static void main(String[] args) {
        ByteBuffer buffer = ByteBuffer.allocate(20);
        //设置可以存储的最大限制量 默认等于容器，最大等于容量。
        buffer.limit(5);
        System.out.println(buffer);   //java.nio.HeapByteBuffer[pos=0 lim=5 cap=20]


    }
}

```

- 获取当前的limit

```java
package com.wnx.model;

import java.nio.*;

public class BufferTest {
    public static void main(String[] args) {
        ByteBuffer buffer = ByteBuffer.allocate(20);
        buffer.limit(5);
        //获取limit
        int limit = buffer.limit();
        System.out.println(limit);//5
    }
}
```


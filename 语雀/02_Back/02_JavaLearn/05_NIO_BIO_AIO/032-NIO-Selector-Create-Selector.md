# NIO-Selector-Create-Selector

```java
package com.wnx.model;

import lombok.SneakyThrows;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.nio.Buffer;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;
import java.nio.channels.Selector;

/**
 * 从目标通道中去复制原通道数据
 */
public class BufferTest {
    @SneakyThrows
    public static void main(String[] args) {
        
       Selector selector = Selector.open();
        System.out.println(selector);   //sun.nio.ch.WindowsSelectorImpl@7c3df479
    }
}
```


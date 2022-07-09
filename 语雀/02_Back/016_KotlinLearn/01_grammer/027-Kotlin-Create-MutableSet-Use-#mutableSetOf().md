# Create-MutableSet-Use-#mutableSetOf()

- 创建一个可变的集合，可以动态进行增加，删除元素。

```kotlin
package com.wnx
fun main() {
    var set:MutableSet<Int> = mutableSetOf(1,2,3,4,5)
    set.add(6)
    set.remove(5)
    
}
```


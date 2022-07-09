# Create-MutableList

- 创建一个可变的集合，可以添加元素，可以修改元素。

```kotlin
package com.wnx
fun main() {
    
   var list:MutableList<Int> =  mutableListOf(1,2,3,4,5)
    
    list.add(1)
    list.add(2)
    list.remove(1)

    
}
```


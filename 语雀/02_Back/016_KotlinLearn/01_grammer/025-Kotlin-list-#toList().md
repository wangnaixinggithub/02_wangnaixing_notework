# list-#toList()

- 将一个可变化的集合变成一个不可变化的集合

```kotlin
package com.wnx
fun main() {

   var list:MutableList<Int> =  mutableListOf(1,2,3,4,5)

    list.add(1)
    list.add(2)
    
    //将可以动态添加元素的集合变成不可变的集合
    val list2 = list.toList()
    
    
}
```


# Kotlin-Create-List-#listOf

- 通过内置方法`#listOf()`可以创建一个集合，并通过`#forEach()` + lambal匿名函数的方式可进行元素遍历。

```kotlin
package com.wnx
fun main() {

    val list:List<Int> = listOf(1,3,4,5,6)
    list.forEach(){ println(it) }
}
```

```
1
3
4
5
6
```


# list-#distinct()

- 集合元素去重

```kotlin
package com.wnx
fun main() {
    var list:List<Int> = listOf(1,2,3,4,5,1,2,3,4,5)
    val distinctList = list.distinct()
    println(distinctList) //[1, 2, 3, 4, 5]
}
```


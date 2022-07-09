# Kotlin-Double-#roundToInt()

- 反正的整数，经过了四舍五入。

```kotlin
package com.wnx

import kotlin.math.roundToInt

fun main() {
   //小数的四舍五入
    val num1:Double = 365.23421
    val num2:Double = 365.76852
    println(num1.roundToInt()) //365
    println(num2.roundToInt()) //366

}
```


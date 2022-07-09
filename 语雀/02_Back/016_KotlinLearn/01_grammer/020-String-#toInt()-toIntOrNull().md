# String-#toInt()-toIntOrNull()

- 将字符串转为整型

```kotlin
package com.wnx

fun main() {
    val str:String = "66"
    val number1 = str.toInt()
    print(number1)
}

```

```
66
```



- 如果输入的值为空，或者输入的字符串不是整数字符串，则转换为null

```java
package com.wnx

fun main() {
    val str:String ?= "66.6"
    val number1 = str?.toIntOrNull()
    print(number1)
}
```

```
null
```


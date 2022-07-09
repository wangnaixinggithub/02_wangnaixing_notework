# Method-Define

- 私有方法的定义，具名方法的定义。

```kotlin
package com.wnx

private fun sum(number1:Int,number2:Int):Int{
    return number1 + number2
}

fun main() {
    val number1 = 10;
    val number2 = 20;

    val sum = sum(number1, number2)
    print("sum = $sum")

}
```

```kotlin
package com.wnx

private fun sum(number1:Int,number2:Int):Int{
    return number1 + number2
}

fun main() {
    val sum = sum(number1=10, number2=20)
    print("sum = $sum")
}
```

```shell
sum=30
```

- 有默认参数的方法定义

```kotlin
package com.wnx

private fun sum(number1:Int=20,number2:Int=20):Int{
    return number1 + number2
}

fun main() {
    val number1 = 10;
    val number2 = 20;

    val sum = sum()
    print("sum = $sum")

}
```

```shell
sum = 40
```




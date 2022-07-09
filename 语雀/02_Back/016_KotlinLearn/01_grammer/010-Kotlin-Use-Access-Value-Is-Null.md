# Use-?-Access-Value-Is-Null

- 默认情况下，声明的变量都不能赋值为`null` 我们可以使用`?` 来限定这种默认，允许变量也可以赋值为`null`

```kotlin
package com.wnx



fun main() {
    var num1:Int ? = 10
    var num2:Float ? = 0.10f
    var num3:Double ? = 0.10
    
    var c1:Char ? = 'w'
    var str1:String ? = "王乃醒是大帅哥！"
    var b1:Boolean ? = false
    
    num1 = null
    num2 = null
    num3 = null
    c1 = null
    str1 = null
    b1 = null
    
}


```


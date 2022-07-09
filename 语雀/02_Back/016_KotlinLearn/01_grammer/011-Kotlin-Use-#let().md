# Use-#let()

#  1、let Invoke Condition is Not Null

- 此代码可以正常运行，但是不会进入到let中，执行`#println()`

```kotlin
package com.wnx

fun main() {

    var str1:String ? = "wangnaixing"
    str1 = null
    str1?.let {
      println("只有str1代码不为空的时候，这let块的逻辑才会执行")
    }

}
```

# 2、it is MySelf

- 里面的it参数总是他本身

```kotlin
package com.wnx
fun main() {
   var str:String = "王乃醒"
    str.let {
        print(it) //王乃醒
    }
}

```

```kotlin
package com.wnx
fun main() {
  var num:Number = 123
    num.let {
        print(it) //123
    }
}
```

```kotlin
package com.wnx
fun main() {
   var list:List<Int> = listOf(1,2,3,4,5,6)
   list.let {
       println(list) //[1, 2, 3, 4, 5, 6]
   }
}
```

# 3、ReturnValue is Your Return

```kotlin
package com.wnx
fun main() {
    var list:List<Int> = listOf(1,2,3,4,5,6)
    var element =  list.let {list[3]}
    println(element)

    var str:String = "王乃醒是一个大帅哥！";
    var str2 = str.let {
        it.substring(0,it.indexOf('醒')+1)
    }
    println(str2)
    
}
```

```shell
4
王乃醒
```


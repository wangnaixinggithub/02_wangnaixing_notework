# Kotlin-Create-Single-Object

> 创建Kotlin单例对象，在整个应用中，只有一个。并且可以直接通过类名调用方法。和Java静态类很相似。

```kotlin
package com.wnx

object ApplicationConfig{



   fun doSomeThing(){
      println("doSomeThing。。。。。。。。。。。。。")
   }
}

fun main() {
   //1.单例对象
   val app = ApplicationConfig
   val app1 = ApplicationConfig
   println(app)
   println(app1)
  
   //2.可以通过类名调用方法
   ApplicationConfig.doSomeThing()

}

//com.wnx.ApplicationConfig@7ea987ac
//com.wnx.ApplicationConfig@7ea987ac
//doSomeThing。。。。。。。。。。。。。


```


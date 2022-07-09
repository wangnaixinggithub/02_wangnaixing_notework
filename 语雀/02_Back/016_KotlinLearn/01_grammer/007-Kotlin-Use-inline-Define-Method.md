# Use-inline-Define-Method

> 类似于C++宏替换，有助于提高性能，底层是直接把`getTwoNumberAndPrint()` 直接变成执行代码放到main()方法中。

```kotlin
package com.wnx



fun main() {

   val sum = getTwoNumberAndPrint(num1 = 20, num2 = 30){
        println("计算两个数之和的结果为${it}")
    }

    println(sum)



}
    
  inline fun  getTwoNumberAndPrint(num1:Int,num2:Int,showMsg:(msg:String)->Unit):Int{
       showMsg((num1+num2).toString());
     return  num1+num2
   }

```


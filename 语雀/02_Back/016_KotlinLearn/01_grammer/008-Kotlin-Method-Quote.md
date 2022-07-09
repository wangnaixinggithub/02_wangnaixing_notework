# Method-Quote	

- 使用`::` 可对方法进行引用

```kotlin
package com.wnx



fun main() {

   val sum = getTwoNumberAndPrint(num1 = 20, num2 = 30,::showMsg)

    println(sum)

}

  inline fun  getTwoNumberAndPrint(num1:Int,num2:Int,showMsg:(msg:String)->Unit):Int{
     showMsg((num1+num2).toString());
     return  num1+num2
   }

  fun  showMsg(str:String){
      println("计算两个数之和的结果为${str}")
  }

```


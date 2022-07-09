# Method-Define-ReturnValue-Is-Method

- Kotlin，接受函数的返回值是一个函数。记得调用`invoke()` 执行他

```javascript
package com.wnx



fun main() {
    //1.调用方法
    val twoNumberAndPrint = getTwoNumberAndPrint(1, 2)
    
    //4.执行此函数
    val str = twoNumberAndPrint.invoke();

}

  //2.方法的返回值是一个函数
  fun  getTwoNumberAndPrint(num1:Int,num2:Int):()->String{
      
      //3.返回匿名函数
      return {
          (num1+num2).toString()
      }
  }


```


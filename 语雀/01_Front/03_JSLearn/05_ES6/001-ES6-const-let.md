# ES6-const-let

> 在ES6中使用let const解决块级作用域问题，let和const有块级作用域，const定义常量，let 定义变量。

# 1、const

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>const的使用</title>
</head>
<body>
<script>
    //1.定义常量，赋值后不能再赋值，在赋值报错
    const count = 1
    count = 2 // Uncaught TypeError: Assignment to constant variable.
</script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>const的使用</title>
</head>
<body>
<script>
    //2.const只声明不赋值会报错，必须赋值
    const count; 
  //Uncaught SyntaxError: Missing initializer in const declaration
</script>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>const的使用</title>
</head>
<body>
<script>
    //常量的含义是你不能改变其指向的对象user，但是你可以改变user属性

    //1.定义一个user常量，并赋值
    const user = {
        name:"王乃醒",
        age:25,
        height:175
    }
    console.log(user)

    //3.给常量赋值
    user.name = "黄洁莹"
    user.age = 22
    user.height = 167

    //4.验证赋值有效
    console.log(user)
</script>
</body>
</html>
```

![image-20220513235615135](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220513235615135.png)

# 2、let

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>02-let的使用</title>
</head>
<body>
<script>
      let user =  {
           name:'王乃醒',
           age:28,
           hobbies: ['代码','香烟','女人'],
           coding:function() {
                 console.log("I LIKE CODING !!")
          }
      }
      let user2
      console.log('User=>',user2)  // User=> undefined
     
    //user user2 共同指同一内存单元
       user2 = user

      console.log('User=>',user2)  // User=> undefined
</script>
</body>
</html>
```

​	![image-20220513235450372](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220513235450372.png)
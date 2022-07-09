# Use-JS-Array-sort()

# 1、number[] orderBy ASC

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>数组sort函数的使用_01_number数组的升序</title>
</head>
<body>
<script>
    //1.定义数组
    let arr = [10,9,8,7,6,5,4,3,2,1]

    console.log("排序之前的数组=>",arr) //排序之前的数组=>  [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]

    //2.执行升序排序
    arr.sort((pre , next ) => pre - next)

    console.log("排序之后的数组=>",arr) //排序之后的数组=>  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

</script>
</body>
</html>
```

# 2、number[] orderBy DESC

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>数组sort函数的使用_02_number数组的降序</title>
</head>
<body>
<script>
    //1.定义数组
    let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

    console.log("排序之前的数组=>",arr) //排序之前的数组=>  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

    //2.执行降序排序
    arr.sort((pre , next ) => next - pre) //todo:注意和升序不用，在于他做差值的值顺序！！！

    console.log("排序之后的数组=>",arr) //排序之后的数组=>  [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]

</script>
</body>
</html>
```

# 3、object[] orderBy ASC

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>数组sort函数的使用_02_object数组的升序</title>
</head>
<body>
<script>
    
   function User(id,name,age){
       this.id = id
       this.name = name
       this.age = age

   }
   

   let userArray = [
       new User(1,'王乃醒',40),
       new User(2,'杨川庆',30),
       new User(3,'杨卫波',20)
   ]


   userArray.sort((pre,next) => pre['age'] - next['age'])


</script>
</body>
</html>
```

```
 另一种写法：  userArray.sort((pre,next) => pre.age - next.age)
```



# 4、object[] orderBy DESC

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>04_数组sort函数的使用_02_数值数组的降序</title>
</head>
<body>
<script>
   function User(id,name,age){
       this.id = id
       this.name = name
       this.age = age
   }

   let userArray = [
       new User(1,'王乃醒',20),
       new User(2,'杨川庆',30),
       new User(3,'杨卫波',40)
   ]


   userArray.sort((pre,next) => next['age'] - pre['age'] )

</script>
</body>
</html>
```

```
 另一种写法  userArray.sort((pre,next) => next.age - pre.age)
```






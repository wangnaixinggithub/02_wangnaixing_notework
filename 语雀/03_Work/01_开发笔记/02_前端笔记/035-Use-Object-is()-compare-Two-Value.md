# Use-Object-is()-compare-Two-Value

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Object类的is方法判断当前值等于判断值吗？</title>
</head>
<body>
<script>
    // 1 等于 1 吗 ?
    console.log(Object.is(1,1)) //true


    //字符串相等吗？
    console.log(Object.is('王乃醒','王乃醒')) // true




    //元素相同的两个数组相等吗？
    let arr1 = [1,2,3,4]
    let arr2 = [1,2,3,4]

    console.log(Object.is(arr1,arr2)) //false 理由：比的是引用


    //数组的内存空间引用值 相等的两个数组相等吗？
    let arr3 = arr1

    console.log(Object.is(arr1,arr3)) //true



    function User(id,name){
        this.id = id
        this.name = name
    }

    //属性值相同，但是内存地址不同的两个对象相等吗？
    let user1 = new User(1,'王乃醒')
    let user2 = new User(1,'王乃醒')
    console.log(Object.is(user1,user2))  //false 理由：比的是引用

    //引用值 相等的两个对象相等吗？
    let user3 = user1
    console.log(Object.is(user1,user3)) //true


</script>
</body>
</html>
```


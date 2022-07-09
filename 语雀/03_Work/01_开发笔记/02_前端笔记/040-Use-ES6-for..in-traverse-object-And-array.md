# Use-ES6-for..in-traverse-object-And-array

- ES6 的 for in 遍历，支持遍历对象，遍历数组。但是并不支持遍历Map

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>11_JS的for-in循环.html</title>
</head>
<body>
<script>
    function User(name,age) {
        this.name = name
        this.age = age
    }

    let user1 = new User('王乃醒',25)
    let user2 = new User('张三',27)
    let objArr = [user1,user2]

    let map = new Map()
    map.set('name','王乃醒')
    map.set('age',25)
    
    //1.遍历对象
    for (let property in user1) {
        console.log("属性名：",property)
        console.log("属性值：",user1[property])

    }


    //2.遍历对象数组
    for (const index in objArr) {
        //通过index，可以拿到对象的值
        console.log("数组索引：",index)
        console.log("数组元素值：",objArr[index])
    }


    // for in 循环 并不能遍历map
    // for (const key in map) {
    //
    // }


</script>
</body>
</html>
```


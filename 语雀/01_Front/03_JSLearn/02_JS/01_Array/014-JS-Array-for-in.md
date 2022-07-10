# for - in

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>09.遍历数组</title>
</head>
<body>
<script>
    function User(name,age){
       this.name = name
       this.age = age
    }

    let user1 = new User('王乃醒',25)
    let user2 = new User('李四',26)
    let user3 = new User('王五',27)

    let userArr = [user1,user2,user3]

    for (let i in userArr){
        console.log('索引 => ',i)
        console.log('元素 => ',userArr[i])
    }

</script>
</body>
</html>
```

![](003-for-in.assets/image-20220514100856024.png)
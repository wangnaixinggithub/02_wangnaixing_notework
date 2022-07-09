# for i

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
     // 下标遍历
    for (let i = 0; i < userArr.length; i++) {
              console.log('user =>',userArr[i])
    }

</script>
</body>
</html>
```

![image-20220514100030726](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220514100030726.png)
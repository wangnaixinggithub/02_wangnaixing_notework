# Array-Method-findIndex

- 搜索符合条件的数组元素索引。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Array-Method-findIndex</title>
</head>
<body>
<script>

    function User(id,name,age){
        this.id = id
        this.name = name
        this.age = age
    }

    let user1 = new User(1,'张三',26)
    let user2 = new User(2,'王乃醒',27)
    let user3 = new User(3,'李四',27)


    //查找符合条件的元素索引，如果找到了就不再向下查找。
    let userArr = new Array(user1,user2,user3)


    //查找年龄为27的用户
    let user = userArr.findIndex(item => Object.is(27,item.age));

    console.log(user) //1


</script>
</body>
</html>
```


# JS-Object-De-Construct

- Js对象的解构，可以从对象中直接获取的属性值。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-Object-De-Construct</title>
</head>
<body>
<script>
    function User(id,name,age,sex){
        this.id = id
        this.name = name
        this.age = age
        this.sex = sex
    }

    let user1 = new User(1,'王乃醒',26,'男')

    const {id,name,age,sex} = user1 || {}

    console.log(id)  //1
    console.log(name)  //'王乃醒'
    console.log(age) //26
    console.log(sex) //'男'

</script>
</body>
</html>
```


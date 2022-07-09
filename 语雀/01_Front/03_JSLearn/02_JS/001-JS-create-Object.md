# JS-create-Object

# 1、{}  .

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS 对象的创建</title>
</head>
<body>
<script>
    let object = {}
    object.id = 1
    object.name = '王乃醒'
    object.tel = '18154622909'

    console.log(typeof object) 

    console.log(object)
</script>
</body>
</html>
```

# 2、{} :

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS 对象的创建</title>
</head>
<body>
<script>
    let object = {
        id:1,
        name:'王乃醒',
        tel:'18154622909'
    }
    
    console.log(typeof object)
    console.log(object)
</script>
</body>
</html>
```

# 3、function new

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS 对象的创建</title>
</head>
<body>
<script>
    function Person(id,name,tel){
        this.id = id
        this.name = name
        this.tel = tel
    }
    let object = new Person(1,'王乃醒','18154622909')


    console.log(typeof object)
    console.log(object)
</script>
</body>
</html>
```

![image-20220529231602929](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220529231602929.png)
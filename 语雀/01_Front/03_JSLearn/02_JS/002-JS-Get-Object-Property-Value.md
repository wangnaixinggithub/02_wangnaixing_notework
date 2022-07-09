# Get-Object-Property-Value

# 1、.

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>getObject</title>
</head>
<body>
<script>
    function Person(id,name,tel){
        this.id = id
        this.name = name
        this.tel = tel
    }
    let object = new Person(1,'王乃醒','18154622909')

    let id = object.id
    let name = object.name
    let tel = object.tel

    console.log(id,name,tel)
</script>
</body>
</html>
```

# 2、[ ]

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>getObject</title>
</head>
<body>
<script>
    function Person(id,name,tel){
        this.id = id
        this.name = name
        this.tel = tel
    }
    let object = new Person(1,'王乃醒','18154622909')

     let id = object['id']
     let name = object['name']
     let tel = object['tel']
    
    console.log(id,name,tel)
</script>
</body>
</html>
```


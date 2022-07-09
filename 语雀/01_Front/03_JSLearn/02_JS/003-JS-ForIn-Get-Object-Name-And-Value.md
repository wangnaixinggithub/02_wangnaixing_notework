# ForIn-Get-Object-Name-And-Value

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>ForIn-Get-Object-Name-And-Value</title>
</head>
<body>
<script>
    function Person(id,name,tel){
        this.id = id
        this.name = name
        this.tel = tel
    }
    let object = new Person(1,'王乃醒','18154622909')

    for (let key in object) {

        console.log("propertyName =>",key)
        console.log("propertyValue => ",object[key])
    }
</script>
</body>
</html>
```


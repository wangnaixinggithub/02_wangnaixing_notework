# JS-Method-Return-Value

- JS如果没有返回值，调用后打印返回值为`undefined`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-Method-Return-Value</title>
</head>
<body>
<script>
    function abc(){
        console.log('aaaa')
    }

    Object.is(undefined,abc()) // true
</script>
</body>
</html>
```


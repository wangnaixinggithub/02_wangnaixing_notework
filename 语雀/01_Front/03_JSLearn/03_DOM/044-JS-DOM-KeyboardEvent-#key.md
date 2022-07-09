# JS-DOM-KeyboardEvent-#key

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>

    </style>
</head>

<body>


<script>
document.onkeydown = (e) =>{
    console.log(e)
    //输出当前触发的是键盘的哪一个按键
    console.log(e.key)
}
</script>
</body>

</html>
```


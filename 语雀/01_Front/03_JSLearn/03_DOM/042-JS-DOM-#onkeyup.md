# JS-DOM-#onkeyup

> 绑定键盘按下的事件。只要键盘某个按钮按下去弹起时就会触发`keyup`事件。

# 1、bind KeyUp By Attr

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
    
document.onkeyup = () =>{
    console.log('按键弹起的时候触发....')
}

</script>
</body>

</html>
```

# 2、bind KeyUp By addEventListener

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
document.addEventListener('keyup',() =>{
    console.log('按键弹起的时候触发....')
})
</script>
</body>

</html>
```


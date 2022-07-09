# JS-DOM-#onkeydown

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
document.onkeydown = () =>{
    console.log('按键按下的时候触发....')
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
document.addEventListener('keydown',() => {
    console.log('按键按下的时候触发....')
})
</script>
</body>

</html>
```

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
    //同样也是按下时触发，不监听ctrl shift 这些按钮...
document.onkeypress = () =>{    
    console.log('keypress...')
}
</script>
</body>

</html>
```


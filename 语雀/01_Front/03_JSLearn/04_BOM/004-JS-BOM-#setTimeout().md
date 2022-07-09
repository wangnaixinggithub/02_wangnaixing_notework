# JS-BOM-#setTimeout()

- 定时器操作。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style></style>
</head>
<script>
    setTimeout(()=>{
        console.log('定时器:两秒钟会触发，只触发一次...')
    },2000)
</script>
<body>

</body>
</html>
```

- 多个定时器

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style></style>
</head>
<script>
    //多个定时器...
    let timer01 = setTimeout(callBack,2000)
    let timer02 = setTimeout(callBack,4000)
    let timer03 = setTimeout(callBack,6000)

    function callBack(){
        console.log('定时器触发一次...')
    }
</script>
<body>

</body>
</html>
```

![image-20220628110237505](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220628110237505.png)
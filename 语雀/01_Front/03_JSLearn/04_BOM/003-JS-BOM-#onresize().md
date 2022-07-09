# JS-BOM-#onresize()

# 1、use Attr

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
    //如果浏览器窗口变化了，就会触发这个事件。
  window.onresize = () =>{
      console.log('当前浏览器窗口大小发生了改变了哈哈...')
  }
</script>
<body>

</body>
</html>
```

# 2、use addEventListener

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
    //如果浏览器窗口变化了，就会触发这个事件。
    window.addEventListener('resize',() =>{
        console.log('当前浏览器窗口大小发生了改变了哈哈...')
    })
</script>
<body>

</body>
</html>
```


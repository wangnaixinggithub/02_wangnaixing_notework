# JS-BOM-#clearTimeout()

- 清空定时器。

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
<body>
<button>拯救地球，阻止地球爆炸。</button>
<script>
    let timer = setTimeout(()=>{console.log('地球爆炸了,哈哈哈哈....')},5000)
    let buttonElement = document.querySelector('button')

    buttonElement.addEventListener('click',()=>{
        clearTimeout(timer)
    })

</script>
</body>
</html>
```


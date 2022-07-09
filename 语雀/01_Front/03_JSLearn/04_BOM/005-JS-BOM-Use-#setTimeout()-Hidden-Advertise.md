# JS-BOM-Use-#setTimeout()-Hidden-Advertise

- 利用定时器，来隐藏广告。

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
<img src="images/ad.jpg" alt="" class="ad">
<script>
    let imgElement = document.querySelector('img')
    setTimeout(() =>{
        imgElement.style.display = 'none'
    },5000)
</script>
<body>

</body>
</html>
```


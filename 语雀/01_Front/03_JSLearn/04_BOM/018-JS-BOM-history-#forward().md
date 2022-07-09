# JS-BOM-history-#forward()

- `history` 对象可以模拟浏览器前进和后退的功能。

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
<button id="btn">前进</button>
<button id="btn2">后退</button>
<script>
    let btnElement = document.querySelector('#btn')
    let btn2Element = document.querySelector('#btn2')

    btnElement.addEventListener('click',()=>{
        history.forward()
    })

    btn2Element.addEventListener('click',()=>{
        history.back()
    })


</script>
</body>
</html>
```


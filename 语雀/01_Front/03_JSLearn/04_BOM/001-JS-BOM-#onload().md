# JS-BOM-#onload()

- JS代码必须在`HTML5`文档解析完成了之后才可以执行！
- window.onload事件，则表明在整个HTML文档解析完成后，就执行该事件。

# 1、Use Attr

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
    window.onload = () =>{
        let btnElement = document.querySelector('#btn')
        btnElement.onclick = () =>{
            alert('我被点击了,哈哈哈...')
        }
    }
</script>
<body>
<button id="btn">按钮</button>
</body>
</html>
```

# 2、Use addEventListener

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
        window.addEventListener('load',() =>{
        let btnElement = document.querySelector('#btn')
        btnElement.onclick = () =>{
            alert('我被点击了,哈哈哈...')
        }
    })
</script>
<body>
<button id="btn">按钮</button>
</body>
</html>
```


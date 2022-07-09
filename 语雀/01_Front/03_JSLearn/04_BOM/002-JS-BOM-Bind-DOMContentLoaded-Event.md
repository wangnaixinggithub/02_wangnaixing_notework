

# BOM-Bind-DOMContentLoaded-Event

- 绑定`DOMContentLoaded` 事件，和`onload` 相似。
- 区别在于，他不用等到所有CSS,图片样式加载完毕后才触发。加载文档后立即生效。
- 适用于，样式资源庞大，图片资源多的情况处置

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
        window.addEventListener('DOMContentLoaded',() =>{
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


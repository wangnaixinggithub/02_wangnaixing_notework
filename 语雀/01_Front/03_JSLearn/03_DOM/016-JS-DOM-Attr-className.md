# JS-DOM-Attr-className

- 默认是使用box1的样式，我们可以使用DOM元素的className属性修改类选择器为.box2

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JS-DOM-Attr-className</title>
</head>
<style>
.box1{
    width: 150px;
    height: 150px;
    background-color: #409EFF;
}

.box2{
    width: 150px;
    height: 150px;
    background-color: red;
}

</style>
<body>
<div class="box1">

</div>
<script>
    
let divElement = document.querySelector('div');
//可以修改类属性，控制样式
divElement.className =  'box2'

</script>
</body>
</html>
```


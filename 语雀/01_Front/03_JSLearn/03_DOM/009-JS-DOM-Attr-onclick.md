# JS-DOM-Attr-onclick

- 给元素绑定点击事件，利用的是DOM元素的`onclick`属性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_dom_attr_onclick</title>
</head>
<body>
<button id="id1">按钮1</button>

</body>
<script>
document.querySelector('#id1').onclick = () => console.log('我被点击了')
</script>
</html>
```


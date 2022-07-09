# JS-Dom-#getElementsByClassName()

- 根据类获取DOM元素。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_dom_#getElementsByClassName()</title>
</head>
<body>
<h3>我的爱好</h3>
<div class="class1">1</div>
<div class="class2">2</div>
<div class="class1">3</div>
<div class="class2">4</div>

</body>
<script>
 let elementsByClassName = document.getElementsByClassName('class1');
 console.log(elementsByClassName)
</script>
</html>
```


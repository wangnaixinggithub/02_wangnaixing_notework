# JS-DOM-#removeAttribute()

- JS提供了removeAttribute() 方法，可以移除到所有属性。不管是元素本身存在的，还是你自己添加的。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_DOM_#removeAttribute()</title>
</head>
<body>
<div id="div" title="hahaha"  index="1" class="divClass">
    <span>hello</span>this is the world
</div>
</body>
<script>
    let divElement = document.querySelector('div')
    
    //1.移除掉自己定义的属性
    divElement.removeAttribute('index')
    divElement.removeAttribute('class')
    
    //2.移除掉内置的属性
    divElement.removeAttribute('title')
    divElement.removeAttribute('id')

</script>
</html>
```


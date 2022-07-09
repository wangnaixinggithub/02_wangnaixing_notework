# JS-DOM-#attributes()

- 获取到DOM元素全部的属性，属性是一个对象。有name value成员变量，对应属性名称，对应属性值。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01_DOM_#attributes()</title>
</head>
<body>
<div id="div" index="1" class="divClass" title="hahaha">
    <span>hello</span>this is the world
</div>
</body>
<script>
    let divElement = document.querySelector('div');

    //获取div元素的所有属性
    let allAttr = divElement.attributes; //得到的是一个对象 NameNodeMap

    //遍历一下
    for (let attr of allAttr) {
        //属性名
        console.log(attr.name)
        //属性值
        console.log(attr.value)
    }
</script>
</html>
```

```js
<script>
    let divElement = document.querySelector('div')
    for (let attribute of divElement.attributes) {
     console.log(attribute.nodeName) //等价于 name
     console.log(attribute.nodeValue) //等价于 value
     console.log(attribute.nodeType) // 属性类型
    }

</script>
```


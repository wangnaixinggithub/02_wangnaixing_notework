# Use-ES6-module-import

> ES6，提出了模块化JS的思想。

## 1、定义一个模块化的JS

```js
//calculator.js
const calcJhxx = (primaryData, jhxxData = []) => {
    console.log("哈哈哈哈")
}



const calculator = {
    calcJhxx
}


export {calculator}

```

## 2、type类型标记为module模块。使用import进行导入。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>计算求和</title>
</head>
<body>

<script type="module">
    import { calculator } from "./calculator.js";
    calculator.calcJhxx("",[])
</script>
</body>
</html>
```

## 3、可见输出

![image-20220507095618361](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220507095618361.png)


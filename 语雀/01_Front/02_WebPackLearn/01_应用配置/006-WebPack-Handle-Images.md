# WebPack-Handle-Images

> 演示处理Images资源

# 1、Need Cofig

```javascript
const path = require("path");

module.exports = {
    entry: "./src/main.js",
    output: {
        path: path.resolve(__dirname, "dist"),
        filename: "main.js",
    },
    module: {
        rules: [
            {
                test: /\.css$/,
                use: ["style-loader", "css-loader"],
            },
            {
                test: /\.less$/,
                use: ["style-loader", "css-loader", "less-loader"],
            },
            {
                test: /\.s[ac]ss$/,
                use: ["style-loader", "css-loader", "sass-loader"],
            },
            {
                test: /\.styl$/,
                use: ["style-loader", "css-loader", "stylus-loader"],
            },
            //处理 png jpeg gif 资源，类型设定为asset
            {
                test: /\.(png|jpe?g|gif|webp)$/,
                type: "asset",
                
                // 小于10kb的图片会被base64处理
                parser: {
                    dataUrlCondition: {
                        maxSize: 10 * 1024
                    }
                }
            },
        ],
    },
    plugins: [],
    mode: "development",
};
```

# 2、Use Images

```javascript
// src/main.js
import $ from 'jquery'
import './css/index.css'
import "./less/index.less"
//1.导入图片
import img from './images/001727MwbEJ.png'

$( ()=>{

    $('li:odd').css('background','pink')
    $('li:even').css('background','red')
    
    //获取img元素，赋值给src属性
    $('img').attr('src',img)
})
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div>
    <ul>
        <li>这是第 1 个 li</li>
        <li>这是第 2 个 li</li>
        <li>这是第 3 个 li</li>
        <li>这是第 4 个 li</li>
        <li>这是第 5 个 li</li>
        <li>这是第 6 个 li</li>
        <li>这是第 7 个 li</li>
        <li>这是第 8 个 li</li>
        <li>这是第 9 个 li</li>
        <li>这是第 10 个 li</li>
    </ul>
</div>

<div class="class1">

</div>

<div class="class2">

</div>
<img  width="300px" height="300px"/>

</body>
</html>
```


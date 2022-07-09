# WebPack-Use-JQuery

# 1、Download

```shell
# 1、生成 pageckage.json文件
npm init -y

# 2、安装 webpack webpack-cli
npm i webpack webpack-cli -D

# 3、安装Jquery
2、npm install jquery 
```

# 2、Impl Tr color Change

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!-- 导入打包之后得到的JS-->
    <script src="/dist/main.js" ></script>
</head>
<body>
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
</ul>
</body>
</html>
```

```java
//src/main.js
import $ from 'jquery'


$( ()=>{
    $('li:odd').css('background','pink')
    $('li:even').css('background','red')
})
```

# 3、Use WebPack Do Pack

```javascript
// 工程目录下/webpack.config.js
const path = require("path")

module.exports = {
    entry: "./src/main.js",
    output: {
        path: path.resolve(__dirname, "dist"),
        filename: "main.js",
    },
    mode: "development",
}
```

配置dev命令,并执行dev脚本命令

```json
{
  "name": "untitled",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
     "dev": "webpack"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^5.73.0",
    "webpack-cli": "^4.10.0"
  },
  "dependencies": {
    "jquery": "^3.6.0"
  }
}
```

# 4、Open index.html In Browser

![image-20220706154149181](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220706154149181.png)
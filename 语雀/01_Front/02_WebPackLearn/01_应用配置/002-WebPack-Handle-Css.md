# WebPack-Handle-Css

> 演示WebPack如何处理CSS资源。

# 1、DownLoad

- 安装css加载器 安装style加载器

```shell
npm i css-loader style-loader -D
```

# 2、Need Config

```javascript
//./config/webpack.dev.js
const path = require("path");

module.exports = {
    entry: "./src/main.js",
    output: {
        path: path.resolve(__dirname, "../dist"),
        filename: "main.js",
    },
    module: {
        rules: [
            //专业处理.css文件，使用样式加载器和css加载器。
            {
                test: /\.css$/,
                use: ["style-loader", "css-loader"],
            },
        ],
    },
    plugins: [],
    mode: "development",
};
```

# 3、Import

```javascript
import sub from "./js/sub"
import sum from "./js/sum"

//引入
import "./css/index.css";

console.log(sub(3,2))
console.log(sum(1,2,3,4,5))
```

# 4、Start

```shell
npx webpack  --config ./config/webpack.dev.js
```


# WebPack-Handle-Less

> 演示WebPack如何处理Less资源。

# 1、Download

```shell
npm install less less-loader --save-dev
```

# 2、Need Config

```java
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
        //处理less资源，使用样式加载器，css加载器，less加载器
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
    ],
  },
  plugins: [],
  mode: "development",
};
```

# 3、Import 

```javascript
import count from "./js/count";
import sum from "./js/sum";
import "./css/index.css";

// 引入资源，Webpack才会对其打包
import "./less/index.less";

console.log(count(2, 1));
console.log(sum(1, 2, 3, 4));
```

# 4、Start

```shell
npx webpack  --config ./config/webpack.dev.js
```


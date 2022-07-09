# Webpack-Handle-Sass-And-Scss

> 演示处理Sass资源以及Scss资源

# 1、DownLoad

```shell
npm i sass-loader sass -D
```

# 2、Need Config

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
        
       //处理Sass Scss资源 ，使用样式加载器 css加载器 sass加载器
      {
        test: /\.s[ac]ss$/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
        
    ],
  },
  plugins: [],
  mode: "development",
};
```

# 3、Import

```javascript
import "../css/index.css";
import "../less/index.less";

// Import
import "../sass/index.sass";
import "../scss/index.scss"
```

# 4、Start

```shell
 npx webpack  --config ./config/webpack.dev.js
```


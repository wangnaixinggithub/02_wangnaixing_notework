# WebPack-Babel

> 使用Babel，能让ES6语法向下兼容。

# 1、DownLoad

- 安装 babel加载器 babel核心依赖，babel-env预设。

```shell
npm i babel-loader @babel/core @babel/preset-env -D
```

# 2、Need Config

```javascript
//babel.config.js

module.exports = {
  presets: ["@babel/preset-env"],
};

```

```javascript
const path = require("path");

//引入 ESLintWebpackPlugin 
const ESLintWebpackPlugin = require("eslint-webpack-plugin");

module.exports = {
  entry: "./src/main.js",
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "static/js/main.js", 
    clean: true, 
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
      {
        test: /\.(png|jpe?g|gif|webp)$/,
        type: "asset",
        parser: {
          dataUrlCondition: {
            maxSize: 10 * 1024, 
          },
        },
        generator: {
          filename: "static/imgs/[hash:8][ext][query]",
        },
      },
      {
        test: /\.(ttf|woff2?|map4|map3|avi)$/,
        type: "asset/resource",
        generator: {
          filename: "static/media/[hash:8][ext][query]",
        },
          
      // 对jS资源进行编译 ES6 => ES5
       {
        test: /\.js$/,
        exclude: /node_modules/, 
        loader: "babel-loader",
      	},    
      },
    ],
  },
  plugins: [
    new ESLintWebpackPlugin({
      context: path.resolve(__dirname, "src"),
    }),
  ],
  mode: "development",
};
```


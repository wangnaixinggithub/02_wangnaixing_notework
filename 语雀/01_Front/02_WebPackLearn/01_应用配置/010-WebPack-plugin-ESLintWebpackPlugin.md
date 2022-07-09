# WebPack-plugin-ESLintWebpackPlugin

> 这是一个JS代码格式检查插件；便于打包时及时发现BUG.

# 1、DownLoad

```shell
npm install eslint-webpack-plugin --save-dev
npm install eslint --save-dev
```

# 2、Need Config

```javascript
//.eslintrc.js
module.exports = {
 
    // 继承 Eslint 规则
  extends: ["eslint:recommended"],
    
  env: {
      
     // 启用node中全局变量
    node: true, 
      
     // 启用浏览器中全局变量  
    browser: true,
  },
    
  parserOptions: {
      
     // ES6 
    ecmaVersion: 6,
      
     // 模块化 
    sourceType: "module",
  },
  rules: {
    // 不能使用 var 定义变量  
    "no-var": 2, 
  },
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
      },
    ],
  },
  plugins: [
      //通过new 构造 ESLintWebpackPlugin
    new ESLintWebpackPlugin({
      // 指定检查文件的根目录
      context: path.resolve(__dirname, "src"),
    }),
  ],
  mode: "development",
};
```


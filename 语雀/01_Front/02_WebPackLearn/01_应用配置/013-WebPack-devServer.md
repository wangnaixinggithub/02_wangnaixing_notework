# WebPack-devServer

> 配置开发服务器，能提供热更新。有利于我们开发。

# 1、DownLoad

```shell
npm i webpack-dev-server -D
```

# 2、Need Config

```javascript
const ESLintWebpackPlugin = require("eslint-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");
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
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: "babel-loader",
            },
        ],
    },
    plugins: [

        new ESLintWebpackPlugin({
            context: path.resolve(__dirname, "src"),
        }),

        new HtmlWebpackPlugin({
            template: path.resolve(__dirname, "public/index.html"),
        }),



    ],
    //开发服务器配置，动态打包项目的内存中。可实现热部署
    devServer: {
        host: "localhost",
        port: "3000",
        open: true,
    },
    mode: "development",
};
```

# 3、New Cmd

```shell
npx webpack serve
```

- 或者在package.json中配置执行脚本dev: webpack serve

```json
{
  "name": "untitled",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack serve"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "webpack": "^5.73.0",
    "webpack-cli": "^4.10.0",
    "webpack-dev-server": "^4.9.3"
  },
  "dependencies": {
    "jquery": "^3.6.0"
  }
}
```


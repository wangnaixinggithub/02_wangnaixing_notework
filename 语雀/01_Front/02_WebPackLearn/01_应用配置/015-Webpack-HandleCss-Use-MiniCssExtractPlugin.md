# Webpack-HandleCss-Use-MiniCssExtractPlugin

> 使用miniCss插件，可以让所有css资源打包在main.css资源中。index.html 通过link方式 去 导入资源样式，解决之前的都是内嵌样式的不足。

# 1、DownLoad

```shell
npm i mini-css-extract-plugin -D
```

# 2、Need Config

- webpack.prod.js 

```javascript
const ESLintWebpackPlugin = require("eslint-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
// 1、 import
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const path = require("path");
module.exports = {
    entry: "./src/main.js",
    output: {
        //生产模式需要输出。
        path: path.resolve(__dirname, "../dist"),
        filename: "static/js/main.js",
        clean: true,
    },
    module: {
        rules: [
            //2、有关 Css 全部用  MiniCssExtractPlugin.loader
            {
                test: /\.css$/,
                use: [MiniCssExtractPlugin.loader, "css-loader"],
            },
            {
                test: /\.less$/,
                use: [MiniCssExtractPlugin.loader, "css-loader", "less-loader"],
            },
            {
                test: /\.s[ac]ss$/,
                use: [MiniCssExtractPlugin.loader, "css-loader", "sass-loader"],
            },
            {
                test: /\.styl$/,
                use: [MiniCssExtractPlugin.loader, "css-loader", "stylus-loader"],
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
            context: path.resolve(__dirname, "../src"),
        }),

        new HtmlWebpackPlugin({
            template: path.resolve(__dirname, "../public/index.html"),
        }),

        new MiniCssExtractPlugin({
            // 3、定义输出文件名和目录
            filename: "static/css/main.css",
        }),


    ],

    mode: "production",
};
```


# WebPack-HtmlWebpackPlugin

> 处理Html资源，能生成一个新的Html文件，不再需要我们手动导入打包后的JS

# 1、DownLoad

```shell
npm install --save-dev html-webpack-plugin
```

# 2、Need Config

```javascript
const ESLintWebpackPlugin = require("eslint-webpack-plugin");

//引入 HtmlWebpackPlugin
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
            // Do: pubilc/index.html COPY dist/index.html
            // JS Auto Import!
            template: path.resolve(__dirname, "public/index.html"),
        }),

    ],
    mode: "development",
};
```

![image-20220529100444585](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220529100444585.png)
# WebPack-Open-Eslint-And-Babel-Cache

> ​	对 Eslint 检查 和 Babel 编译结果进行缓存。

# 1、Need Config

```javascript
const ESLintWebpackPlugin = require("eslint-webpack-plugin");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const path = require("path");
module.exports = {
    entry: "./src/main.js",
    output: {
        path: undefined,
        filename: "static/js/main.js",
    },
    module: {
        rules: [
            {
                oneOf: [
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
                        include: path.resolve(__dirname, "../src"),
                        loader: "babel-loader",
                        //2、Babel 缓存 并不进行压缩缓存。
                        options: {
                            cacheDirectory: true,
                            cacheCompression: false,
                        },
                    },
                ],
            }
        ]
    },
    plugins: [

        new ESLintWebpackPlugin({
            context: path.resolve(__dirname, "../src"),
            exclude: "node_modules",
            //1、Eslint开启缓存，并设置缓存目录
            cache: true, 
            cacheLocation: path.resolve(__dirname, "../node_modules/.cache/.eslintcache"),
        }),

        new HtmlWebpackPlugin({
            template: path.resolve(__dirname, "../public/index.html"),
        }),


    ],
    devServer: {
        host: "localhost",
        port: "3000",
        open: true,
        hot: true
    },
    mode: "development",
    //开发配置：cheap-module-source-map
    devtool: "cheap-module-source-map",
};
```


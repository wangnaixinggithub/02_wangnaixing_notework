# WebPack-Include-exclude

> 注明，打包JS时，应当只打包哪些模块的JS.   注明ESlint检查时，不检查哪些模块的JS

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
                        //只编译src下的JS
                        include: path.resolve(__dirname, "../src"),
                        loader: "babel-loader",
                    },
                ],
            }
        ]
    },
    plugins: [

        new ESLintWebpackPlugin({
            context: path.resolve(__dirname, "../src"),
            //处理node_modules模块的ES语法不检查其他都检查。
            exclude: "node_modules",
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


# WebPack-Use-Clean

> 演示Webpack开启自动清空打包后得到dist目录里面的文件。

```javascript
const path = require("path");

module.exports = {
    entry: "./src/main.js",
    output: {
        path: path.resolve(__dirname, "dist"),
        filename: "static/js/main.js",
        // 自动将上次打包目录资源清空
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
                    filename: "static/images/[hash:8][ext][query]",
                },
            },
        ],
    },
    plugins: [],
    mode: "development",
};
```


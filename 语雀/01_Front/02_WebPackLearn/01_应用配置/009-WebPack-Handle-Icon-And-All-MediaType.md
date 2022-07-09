# WebPack-Handle-Icon-And-All-MediaType

> 演示处理Icon图标资源和音频、视频等资源。

# 1、DownLoad

```properties
阿里巴巴矢量库：https://www.iconfont.cn/
```

# 2、Need Style

![image-20220529090813437](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220529090813437.png)

# 3、Import

```shell
import sub from "./js/sub"
import sum from "./js/sum"
import "./css/index.css"
import "./less/index.less"
import "./sass/index.sass"
import "./scss/index.scss"
import "./styl/index.styl"
//导入
import "./font/iconfont.css"

console.log(sub(3,2))
console.log(sum(1,2,3,4,5))
```

# 4、Need Config

```javascript
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
                    filename: "static/images/[hash:8][ext][query]",
                },
            },
            //处理图标资源,类型为asset/resource
            {
                test: /\.(ttf|woff2?)$/,
                type: "asset/resource",
                generator: {
                    filename: "static/media/[hash:8][ext][query]",
                },
            },
        ],
    },
    plugins: [],
    mode: "development",
};
```

# 5、Handle map4 map3 avi

```javascript
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
                    filename: "static/images/[hash:8][ext][query]",
                },
            },
            //处理资源加上mp4 mp3 avi
            {
                test: /\.(ttf|woff2?|map4|map3|avi)$/,
                type: "asset/resource",
                generator: {
                    filename: "static/media/[hash:8][ext][query]",
                },
            },
        ],
    },
    plugins: [],
    mode: "development",
};
```


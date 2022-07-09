# 001-Webpack-Install-Start+

> 演示初始化一个node.js工程，并使用Webpack进行工程打包。

# 1、DownLoad

- 初始化`package.json`,  执行后会自动生成package.json。

```shell
npm init -y
```

- 下载`webpack-cli`

```shell
npm i webpack webpack-cli -D
```

# 2、Start

- 打包src目录下面的main.js文件， 打包时会把main.js依赖的js 一起打包。

```shell
npx webpack ./src/main.js --mode=development
```

```shell
npx webpack ./src/main.js --mode=production
```

`--mode=xxx`：指定模式（环境）。

# 3、Output

- 默认 `Webpack` 会将文件打包输出到 `dist` 目录

![image-20220528173037223](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220528173037223.png)
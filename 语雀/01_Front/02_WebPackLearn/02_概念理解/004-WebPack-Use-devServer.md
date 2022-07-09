# WebPack-Use-devServer

webpack-dev-server 会启动一个实时打包的 http 服务器,打包JS代码后生成的文件`bundle.js` 会放在内存中。

# 1、bundle.js In Mamery

> 打包生成的文件哪儿去了？

不配置 webpack-dev-server 的情况下，webpack 打包生成的文件，会存放到实际的物理磁盘上:

- 严格遵守开发者在 webpack.config.js 中指定配置
- 根据 output 节点指定路径进行存放

配置了 webpack-dev-server 之后，打包生成的文件存放到了内存中:

-  不再根据 output 节点指定的路径，存放到实际的物理磁盘上
- 提高了实时打包输出的性能，因为内存比物理磁盘速度快很多

# 2、Resources Access

>  生成到内存中的文件该如何访问？

webpack-dev-server 生成到内存中的文件，默认放到了项目的根目录中，而且是虚拟的、不可见的。

- 可以直接用 / 表示项目根目录，后面跟上要访问的文件名称，即可访问内存中的文件，比如：

```properties
http://localhost:3000/bundle.js
http://localhost:3000/index.html
```


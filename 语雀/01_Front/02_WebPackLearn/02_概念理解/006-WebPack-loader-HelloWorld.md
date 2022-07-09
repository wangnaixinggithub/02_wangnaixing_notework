# WebPack-loader-HelloWorld

# 1、WebPack Only Handle JS

在实际开发过程中，webpack 默认只能打包处理以` .js `后缀名结尾的模块。

。其他非` .js `后缀名结尾的模块，` webpack `默认处理不了，需要调用` loader 加载器`才可以正常打包，否则会报错！

# 2、Loader Can Do

`loader` 加载器的作用：协助 `webpack `打包处理特定的文件模块。比如：

- ` css-loader `可以打包处理` .css `相关的文件
- `less-loader` 可以打包处理 `.less `相关的文件
- `babel-loader` 可以打包处理 `webpack` 无法处理的`高级 JS 语法`

# 3、Loader Process

loader 的调用过程：流程图

![image-20220706225326670](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220706225326670.png)

loader 的调用过程：代码层面

![image-20220707091715782](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220707091715782.png)

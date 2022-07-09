# WebPack-Source Map

# 1、生产环境遇到的问题

前端项目在投入生产环境之前，都需要对 JavaScript 源代码进行压缩混淆，从而减小文件的体积，提高文件的 加载效率。此时就不可避免的产生了另一个问题：

对压缩混淆之后的代码除错（debug）是一件极其困难的事情

![image-20220706232953811](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220706232953811.png)

- 变量被替换成没有任何语义的名称
- 空行和注释被剔除

# 2、什么是 Source Map

Source Map 就是一个信息文件，里面储存着位置信息。也就是说，Source Map 文件中存储着压缩混淆后的代码，所对应的转换前的位置。

有了它，出错的时候，除错工具将直接显示原始代码，而不是转换后的代码，能够极大的方便后期的调试。






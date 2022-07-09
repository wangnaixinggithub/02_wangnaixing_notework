# 001-Install-MongoDB-Software

# 1、Download

- 选择`Windows x64`版本下载，打包方式为msi .下载地址：https://www.mongodb.com/download-center/community

![](D:\my_notebook\typora笔记图片统一管理处\QQ截图20211206142854-16394682122711.jpg)

# 2、Installation

- Choose Defalut Path

![](D:\my_notebook\typora笔记图片统一管理处\QQ截图20211206143519-16394682303253.jpg)

- Run Service as NetWork Service 

![](D:\my_notebook\typora笔记图片统一管理处\QQ截图20211206143948-16394682480795.jpg)

- Not Install MongoDB Compass

![](D:\my_notebook\typora笔记图片统一管理处\QQ截图20211206144004-16394682688297.jpg)

# 3、Start

- 双击`mongo.exe`可以运行MongoDB自带客户端，操作MongoDB；

![](D:\my_notebook\typora笔记图片统一管理处\QQ截图20211206144226-16394682852709.jpg)

- Show Success Msg

![](D:\my_notebook\typora笔记图片统一管理处\QQ截图20211206144307-163946829747611.jpg)

# 4、End

- 在CMD命令窗口，可以使用此命令，停止MongoDB服务。

```
$ sc.exe delete MongoDB
```

![](001-Install-MongoDB-Software.assets/QQ%E6%88%AA%E5%9B%BE20211206144431-163946832042713.jpg)
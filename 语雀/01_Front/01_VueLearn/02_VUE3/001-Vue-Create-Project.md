# Vue-Create-Project

# 1、Install Vue Project By Cmd

```properties
#1、确保本机已经安装好了Vue-cli 

#2、使用Vue create命令创建Vue工程
vue create 03_vue3_learn

#3、选取自定义预设

#4、勾选 abelJS兼容性处理、TypeScript,Css预处理器

#5、选择Vue版本为 3.x

#6、使用class风格的组件语法

#7、使用Babel与TypeScript一起用于自动检测的填充

#8、选择Less样式预处理

#9、Eslint进行报错提醒

#10、TypeScript 检查 在保存时检测  Lint On Save

#11、使用独立的配置文件

#12、保存当前的预设，在以后你二次创建VUE工程时使用

#13、输入当前预设名称 wangnaixing_vue3_preset

#14、进入到工程目录
cd  03_vue3_learn

#15、执行Vue项目运行指令
npm run serve

#16、加载完毕会得到一个网址，http://localhost:8080 代表使用vue-cli创建vue3项目成功了！

```

# 2、Install Vue Project By UI

```shell
# 1、打开VUE图像化创建工程界面
vue ui
```

- 选择VUE项目的文件路径，以及项目文件夹名称。

<img src="C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220710152532630.png" alt="image-20220710152532630" style="zoom:50%;" />

- 选取自定义Vue 工程预设。

<img src="C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220710152746536.png" alt="image-20220710152746536" style="zoom:80%;" />

- 勾选 、BabelJS兼容性处理、TypeScript 语法支持，Css预处理器需要自定义设置 ,选择生成独立的配置文件.TypeScript 检查 在保存时检测  Lint On Save

<img src="C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220710152830006.png" alt="image-20220710152830006" style="zoom: 80%;" />

- 选择Vue版本为 3.x ，使用class-style component, 使用Babel的TypeScript进行处理， 选择Less预处理器配置，

![image-20220710153041615](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220710153041615.png)

- 创建项目，不保存本次预设。

# 3、Use Vite Create

```shell
#1、使用Vite命令创建Vue工程
npm init vite@latest 03_vue3_learn -- --template vue

#2、选择VUE框架

#3、选择vue-ts

#4、进入工程目录
cd  03_vue3_learn

#5、安装Npm 依赖
npm install

#6、执行Vue项目运行指令
npm run dev

#16、加载完毕会得到一个网址，http://localhost:3000 代表使用vue-cli创建vue3项目成功了！

```


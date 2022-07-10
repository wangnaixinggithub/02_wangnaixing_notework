# Vue-HelloWorld

# 1、什么是 VUE

官方给出的概念：Vue (读音` /vjuː/`，类似于 view) 是一套用于构建用户界面的前端框架。

![image-20220706235210921](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220706235210921.png)



# 2、VUE 的特性

VUE 框架的特性，主要体现在如下两方面：

- 数据驱动视图

在使用了 VUE的页面中，VUE会监听数据的变化，从而自动重新渲染页面的结构。示意图如下：

![image-20220706235259866](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220706235259866.png)

好处：当页面数据发生变化时，页面会自动重新渲染！

注意：数据驱动视图是单向的数据绑定。

- 双向数据绑定

在填写表单时，双向数据绑定可以辅助开发者在不操作 DOM 的前提下，自动把用户填写的内容同步到数据源 中。示意图如下：

![image-20220706235341114](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220706235341114.png)

好处：开发者不再需要手动操作 DOM 元素，来获取表单元素最新的值

-  MVVM

MVVM 是 VUE实现数据驱动视图和双向数据绑定的核心原理。MVVM 指的是 Model、View 和 ViewModel， 它把每个 HTML 页面都拆分成了这三个部分，如图所示：

![image-20220706235452050](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220706235452050.png)

MVVM 的工作原理:

ViewModel 作为 MVVM 的核心，是它把当前页面的数据源（Model）和页面的结构（View）连接在了一起。

![image-20220706235542865](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220706235542865.png)

当数据源发生变化时，会被 ViewModel 监听到，VM 会根据最新的数据源自动更新页面的结构 

当表单元素的值发生变化时，也会被 VM 监听到，VM 会把变化过后最新的值自动同步到 Model 数据源中

# 3、VUE的版本

当前，VUE共有 3 个大版本，其中：

- 2.x 版本的 VUE是目前企业级项目开发中的主流版本
- 3.x 版本的 VUE于 2020-09-19 发布，生态还不完善，尚未在企业级项目开发中普及和推广
- 1.x 版本的 VUE几乎被淘汰，不再建议学习与使用

总结： 3.x 版本的 VUE是未来企业级项目开发的趋势； 2.x 版本的 VUE在未来（1 ~ 2年内）会被逐渐淘汰；




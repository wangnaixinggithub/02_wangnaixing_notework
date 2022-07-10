# Vue-components-Use-Component

# 1、使用私有组件

 通过 components 注册的是私有子组件

![image-20220708110135055](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708110135055.png)

例如： 在组件 A 的 components 节点下，注册了组件 F。

 则组件 F 只能用在组件 A 中；不能被用在组件 C 中

# 2、使用全局组件

注册全局组件，在 vue 项目的 main.js 入口文件中，通过 Vue.component() 方法，可以注册全局组件。示例代码如下：

![image-20220708110446367](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708110446367.png)

直接以标签形式使用就可以
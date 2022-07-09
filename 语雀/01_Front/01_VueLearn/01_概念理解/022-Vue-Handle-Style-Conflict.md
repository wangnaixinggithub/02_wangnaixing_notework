# Vue-Handle-Style-Conflict

组件之间的样式冲突问题

默认情况下，写在 .vue 组件中的样式会全局生效，因此很容易造成多个组件之间的样式冲突问题.

导致组件之间样式冲突的根本原因是：

- 1、单页面应用程序中，所有组件的 DOM 结构，都是基于唯一的 index.html 页面进行呈现的

- 2、每个组件中的样式，都会影响整个 index.html 页面中的 DOM 元素



思考：如何解决组件样式冲突的问题

# 1、Scoped

为了提高开发效率和开发体验，vue 为 style 节点提供了 scoped 属性，从而防止组件之间的样式冲突问题

![image-20220708142027784](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708142027784.png)

# 2、Assign Unique Attribute

为每个组件分配唯一的自定义属性，在编写组件样式时，通过属性选择器来控制样式的作用域

![image-20220708142138105](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708142138105.png)
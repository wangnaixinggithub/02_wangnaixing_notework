# Vue-components-script-data

# 1、Data Must be A Function

> .vue 组件中的 data 必须是函数

vue 规定：.vue 组件中的 data **必须是一个函数**，不能直接指向一个数据对象

因此在组件中定义 data 数据节点时，下面的方式是错误的!

![image-20220708103529422](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708103529422.png)

# 2、The Official Sample

使用对象带来的问题：会导致**多个组件实例**共用**同一份数据**的问题

```properties
例子： https://cn.vuejs.org/v2/guide/components.html#data-必须是一个函数
```

![image-20220708103903801](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708103903801.png)
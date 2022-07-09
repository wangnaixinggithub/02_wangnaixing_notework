# Vue-Component-Communication-Child-#prop-default

# 1、Default Prop Value Is undefined

> 使用了prop，而父组件没有通信子组件，传递通信值。那么就这个值就是undefined

![image-20220708134149807](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708134149807.png)

# 2、Use Prop#default Give A Default Value

在声明自定义属性时，可以通过 default 来定义属性的默认值。

![image-20220708134346141](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708134346141.png)

```vue
<template>
<div>
  我是Count组件
  <button @click="btnCalcClickHandle">+1</button>
  {{count}}
</div>
</template>

<script>
export default {
  props:{
    init:{
      //如果父组件没有传值，那么Vue会给init一个默认值1
      default:1
    }
  },

  name: "Count",
  data(){
   return{
     count:this.init
   }
  },
  methods:{
    btnCalcClickHandle(){
      this.count++
    }
  }
}
</script>

<style scoped>

</style>
```

# 3、Validation Change

可以看到 `init `  不再是 undefined 而是 默认值 1

![image-20220708134434434](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708134434434.png)
# Vue-Component-Communication-Child-#prop

父组件向子组件传递数据，可以在子组件中定义prop属性，然后在父组件使用子组件时，注明通信的传输值。

# 1、Define Prop In Child Vue

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
  //1、定义接受父组件的值
  props:['init'],
  
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

![image-20220708131845897](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708131845897.png)

# 2、Parent Vue Communication Children Vue

```vue
<template>
<div>
  我是Left组件
  <!--父组件向子组件通信  -->
  <Count :init="9"></Count>
</div>
</template>

<script>
export default {
  name: "Left"
}
</script>

<style scoped>

</style>
```


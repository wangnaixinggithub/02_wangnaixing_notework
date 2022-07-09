# Vue-Component-Communication-Child-#prop-type

`props` 的 `type` 值类型 在声明自定义属性时，可以通过 `type` 来定义属性的值类型。

![image-20220708135043784](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708135043784.png)

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
      default:1,
      //规定用户传过来的值必须是Number类型
      type:Number
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


# Vue-Component-Communication-Child-#prop-require

在声明自定义属性时，可以通过 required 选项，将属性设置为必填项，强制用户必须传递属性的值

![image-20220708135353195](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220708135353195.png)

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
      type:Number,
      //必填项校验，规定了用户必须传递。
      require:true
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


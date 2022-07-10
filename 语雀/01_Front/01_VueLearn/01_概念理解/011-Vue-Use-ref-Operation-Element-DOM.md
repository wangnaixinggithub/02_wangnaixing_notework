# Vue-Use-ref-Operation-Element-DOM

使用ref来得到元素DOM，并可以进行DOM操作。

# 1、ref And this.$refs

```vue
<template>
  <div id="app">
    <!--1、给元素加入ref属性 -->
    <div ref="div01" style="height: 200px;width: 200px; border: 1px solid red;"></div>
    <button @click="setDivColor">设置DIV背景色</button>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
      count:1,
    }
  },
  components: {

  },
  methods:{

    setDivColor(){
      //2、this.$refs是一个全局定义下的对象，此对象的属性下,key=ref value=元素DOM
      console.log(this.$refs)
      //3、获取到DIV元素
      let divElement = this.$refs['div01']
      console.log(divElement)
      //4、设置DIV的背景色为红色
      divElement.style.backgroundColor = 'red'
    }
  }

}
</script>

<style lang="less">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

![image-20220710095529168](C:/Users/wangnaixing/AppData/Roaming/Typora/typora-user-images/image-20220710095529168.png)






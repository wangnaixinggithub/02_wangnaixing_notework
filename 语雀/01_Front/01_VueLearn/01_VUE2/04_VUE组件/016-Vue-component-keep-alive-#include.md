# Vue-component-keep-alive-#include

```vue
  <template>
    <div id="app">
        <div>
          <button @click="componentName='One'">One</button>
          <button @click="componentName='Two'">Two</button>
          <button @click="componentName='Three'">Three</button>
        </div>
         <div>
          <!--include用来指定哪些组件需要缓存处理的，默认不指定为全部处理 输入的One,Two是组件名称-->
           <keep-alive include="One,Two">
             <component :is="componentName"></component>
           </keep-alive>
         </div>

    </div>
  </template>

  <script>
  import One from "@/components/One"
  import Two from "@/components/Two"
  import Three from "@/components/Three"


  export default {
    name: 'App',
    components:{
      One,
      Two,
      Three
    },
    data(){
      return {
         componentName:''
      }
    },

    methods: {



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


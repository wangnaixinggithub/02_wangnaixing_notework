# Vue-component-component-#is

vue 提供了一个内置的组件，专门用来实现动态组件的渲染。

```vue
  <template>
    <div id="app">
      <!--通过is属性，确定当前要渲染的组件名称-->
      <component :is="componentName"></component>
      
      
      
      <button @click="componentName='left'">渲染组件左组件</button>
      <button @click="componentName='right'">渲染组件右组件</button>
    </div>
  </template>

  <script>



  import Left from "@/components/Left";
  import Right from "@/components/Right";
  export default {
    name: 'App',
    components:{
      Right,
      Left

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


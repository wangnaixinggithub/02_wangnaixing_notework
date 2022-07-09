# Vue-Use-Echarts

```
https://echarts.apache.org/examples/zh/editor.html?c=pie-borderRadius
```

# 1、Need Config

- download

```shell
npm install axios --save
npm install echarts --save
```

- autoward main.js

```javascript
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

//1、导入
import axios from 'axios'
import * as echarts from 'echarts'


Vue.config.productionTip = false

//2、作为Vue的全局变量
Vue.prototype.$axios = axios
Vue.prototype.$echarts = echarts


new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```



# 2、Use It

```vue
<template>
  <div class="home">
    <h3>这是echarts图像等下展示的地方</h3>
    <div id="myChart" :style="{ width: '700px', height: '700px' }"></div>
  </div>
</template>

<script>

export default {
  name: "Home",

  methods: {
    data(){
        return {
          data:[]
        }
    },
    draw() {
        
      //1、根据Dom位置初始化Echart实例    
      let myChart = this.$echarts.init(document.getElementById("myChart"));
      
      let chartOption = {
            
            tooltip: {
              trigger: "item",
            },

            legend: {
              top: "5%",
              left: "center",
            },

            series: [
              {
                name: "Access From",
                type: "pie",
                radius: ["40%", "70%"],
                avoidLabelOverlap: false,
                itemStyle: {
                  borderRadius: 10,
                  borderColor: "#fff",
                  borderWidth: 2,
                },
                label: {
                  show: false,
                  position: "center",
                },
                emphasis: {
                  label: {
                    show: true,
                    fontSize: "40",
                    fontWeight: "bold",
                  },
                },
                labelLine: {
                  show: false,
                },
                data:this.data,
              },
            ],
      }
        
      //2、注入实例配置  
      myChart.setOption(chartOption)
          
    },
      
  },
        
  mounted(){
      
    this.$axios.get('http://localhost:9000/book/findAll').then(res=>{
      let bookList = res.data.data;
   
      let data = [];
      
      for(let i =0;i<bookList.length;i++){
        let element = {name:'',value:0}
          
        element.name = bookList[i].name
        element.value = bookList[i].number
        data.push(element)
            
      }
        
      data.sort((a,b)=>b.value-a.value)
          
      this.data = data;
    
        this.draw();
    }).catch(err=>{
      console.log(err);
      alert("请求失败！请检查后端接口！！");
    });
  }
};
</script>

```

# 4、效果

![image-20220109225432992](D:/%E5%9B%BE%E7%89%87%E7%BB%9F%E4%B8%80%E4%BF%9D%E7%AE%A1%E5%A4%84/image-20220109225432992.png)
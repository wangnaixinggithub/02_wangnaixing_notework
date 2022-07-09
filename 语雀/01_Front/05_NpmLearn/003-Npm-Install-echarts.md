# Npm-Install-echarts

# 1、Download

```shell
npm install echarts --save
```

# 2、Use It

- main.js

```javascript
#main.js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'
import axios from 'axios'
//1.导入
import * as echarts from 'echarts'


Vue.config.productionTip = false
Vue.prototype.$axios = axios

//2.定义为Vue的全局变量
Vue.prototype.$echarts = echarts



new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```


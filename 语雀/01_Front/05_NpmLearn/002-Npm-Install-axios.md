# Npm-Install-axios

# 1、Download

```shell
npm install axios --save
```

# 2、Use It

- main.js

```javascript
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

//1、导入 axios
import axios from 'axios'
import * as echarts from 'echarts'


Vue.config.productionTip = false
//2、定义为Vue的全局变量。名称为$axios
Vue.prototype.$axios = axios
Vue.prototype.$echarts = echarts


new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```


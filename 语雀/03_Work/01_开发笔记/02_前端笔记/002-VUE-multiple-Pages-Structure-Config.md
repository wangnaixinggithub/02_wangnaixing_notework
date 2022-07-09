# VUE-multiple-Pages-Structure-Config



# 1、App.vue

```vue
<template>
  <div id="app">
      <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
    @import "../../common/css/main.css";
    
    #app {
      font-family: 'Avenir', Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      text-align: center;
      color: #2c3e50;
      margin-top: 10px;
    }
    
    ::-webkit-scrollbar {
        width: 8px;
        height: 8px;
        background-color: #fff;
    }
    
    ::-webkit-scrollbar-thumb {
        -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, .3);
        background-color: rgba(0, 0, 0, .1);
        border-radius: 2em;
    }
    
</style>

```

# 2、azsbjx.html

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>安自装置检修流程</title>
</head>
<body>
<!-- build files will be auto injected -->
<div id="app"></div>
</body>
</html>
```

# 3、azsbjx.js

```javascript
import Vue from 'vue'
import App from './App'
import router from './router'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
import http from "../../common/utils/http";
import tools from "../../common/utils/tools"
import eventBus from '../../common/utils/eventBus.js';
import registerPluges from '../platform/utils/register.plugins.js';
import dateUtils from "./api/dateUtils.js";
import cache from "./utils/cache.js";
Vue.config.productionTip = false

Vue.use(ElementUI);
Vue.use(registerPluges);

Vue.prototype.$http = http;
Vue.prototype.tools = tools;
Vue.prototype.$cache = cache;
Vue.prototype.dateUtils = dateUtils;
Vue.prototype.$eventBus = eventBus;
Vue.prototype.windowClose = ()=>{ window.close(); }


new Vue({   
    el: '#app',
    router,
    components: { App },
    template: '<App/>'
})

```

# 4、router.js

```javascript
import Vue from 'vue'
import Router from 'vue-router'


import index from './views/Index.vue'
import add from './views/Add.vue'


Vue.use(Router);


export default new Router({
    routes: [
        {
            path: '/',
            redirect: "/index",
        },
        {
            path: '/index',
            name: 'index',
            component: index,
            props: (route)=>({ params: route.query }),
        },
        {
            path: '/add',
            name: 'add',
            component: add,
            props: (route)=>({ params: route.query }),
        }
    ]
})
```


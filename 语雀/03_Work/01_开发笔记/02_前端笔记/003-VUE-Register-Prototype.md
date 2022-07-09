# VUE-Register-Prototype

# 1、cache.js

```javascript
const cache = {
    //检修类别
    jxlbOptions:[
        "计划检修",
        "临时检修",
        "紧急抢修",
        "新设备投产"
    ],
};
export default cache;
```

# 2、Main.js

```javascript
import cache from "./utils/cache.js";
Vue.prototype.$cache = cache;
```

# 3、this.$cache

```vue
    <el-select v-model="value" placeholder="请选择">
        <el-option
            v-for="item in $cache.jxlbOptions"
            :key="item"
            :label="item"
            :value="item">
        </el-option>
    </el-select>
```


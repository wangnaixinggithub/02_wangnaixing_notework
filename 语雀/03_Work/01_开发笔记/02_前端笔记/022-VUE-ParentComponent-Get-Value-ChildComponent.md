# VUE-ParentComponent-Get-Value-ChildComponent

# 1、ChildComponent Import

```java
<script>
import yxydjh from "../views/yxydjh/yxydjh";
import cfyj from "../views/yjxx/cfyj";

export default {
  name: "Tabs",
  props: ['parameters'],

  components: {
    yxydjh,
    cfyj
  },

</script>
```

# 2、GET Value

```vue
<template>
  <div id="tabs">
      <el-tabs v-model="activeName" :active-name="activeName" type="card" @tab-click="handleClick">
       
        <el-tab-pane label="有序用电计划" name="有序用电计划" :closable="true" class="panel">
            <yxydjh 
                    :parameters="parameters" 
                    :parentData="parentData" 
                    :isTab="true" 
                    @getFormObjData="getFormObjData">
            </yxydjh>
        </el-tab-pane>
          
        <el-tab-pane label="错峰预警" name="错峰预警" :closable="true" class="panel">
            <cfyj :parameters="parameters" 
                  :parentData="parentData" 
                  :isTab="true" 
                  @getFormObjData="getFormObjData">
            </cfyj>
        </el-tab-pane>
          
      </el-tabs>
  </div>
</template>
<script>
  props: ['parameters'],
  data () {
    return {
      activeName: '有序用电计划',
      parentData: {
        formData: {
          primaryData: [{}], // 主表信息
          jhData: [], // 有序用电计划
          yjxxData: []// 有序用电预警信息
        },
        oldFormData: {}
      }
    }
  },
</script>
```

# 3、Child Definition prop

```vue
<template>
<div>
  <h2>有序用电计划</h2>
</div>
</template>

<script>
export default {
  name: "yxydjh",
  mounted() {
    console.log('得到父组件传过来的值',this.parameters,this.isTab,this.parentData)
  },
    
  props: {
      
    parameters: {
      type: Object,
      default: () => {
        return {}
      }
    },
      
    isTab: {
      type: Boolean,
      default: false
    },
      
    parentData: {
      type: Object,
      default: () => {
        return {
          formData: {
            primaryData: [{}], // 主表信息
            jhData: [], // 有序用电计划
            yjxxData: []// 有序用电预警信息
          },
          oldFormData: {}
        }
      }
        
    },
  }
}
</script>


<style scoped>

</style>
```

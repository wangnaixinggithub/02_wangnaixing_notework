# ChildComponent-invoke-ParentComponent-Method

# 1、`@getFormObjData="getFormObjData"`

```vue
//父
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
            <cfyj 
            :parameters="parameters" 
            :parentData="parentData"
            :isTab="true" 
            @getFormObjData="getFormObjData"></cfyj>
        </el-tab-pane>
      </el-tabs>
    <el-button @click="printDataForm">打印现在数据模型的值</el-button>
  </div>
</template>
<script>
 methods:{
    getFormObjData (){
      console.log("我被调用了！！")
  }
}
</script>
```

# 2、`this.$emit`

```vue
//子
<template>
<div>
  <h2>有序用电计划</h2>
  <el-button @click="getFormObjData">修改父组件定义的数据模型</el-button>
</div>
</template>

<script>
export default {
  name: "yxydjh",
  methods:{
    getFormObjData () {
      this.$emit('getFormObjData')
    }
  }
}
</script>

<style scoped>

</style>
```


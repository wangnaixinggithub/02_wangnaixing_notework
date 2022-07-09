# 031-ParentComponent-Get-ChildComponent-Value

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
export default {
  name: "Tabs",
 methods:{
    getFormObjData (item){
      console.log("我被调用了！！")
      console.log(item) //这是子组件传来的值哦！！  
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
  mounted() {
    console.log('得到父组件传过来的值',this.parameters,this.isTab,this.parentData)
  },
  data(){
    return {
      user: {name:'王乃醒',age:26}
    }
  },
  methods:{
    getFormObjData () {
      this.$emit('getFormObjData',this.user)
    }
  }
}
</script>

<style scoped>

</style>
```


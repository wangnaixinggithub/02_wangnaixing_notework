# VUE-computed-split-V-Model

# 1、computed#primaryData#get#set

```js
<script>
export default {
  name: 'jbxx',
  props: ['parameters'],
   data () {
    return {
      formData: {
        primaryData: [{}], // 主表信息
        jbxxData: [{'SJ': ''}]// 有序用电基本信息
      }, 	 
    },
        
 computed: {
   primaryData: {
       get: function () {
        return this.formData.primaryData[0]
      },
          
      set: function (val) {
        this.formData.primaryData[0] = val
      }
       
    },
  }	
}
</script>
```

# 2、Split get V-Model Filed

```javascript
<script>
export default {
  name: 'jbxx',
  data () {
    return {
      formData: {
        primaryData: [{}], // 主表信息
        jbxxData: [{'SJ': ''}]// 有序用电基本信息
      }, 	 
    },
      
 computed: {
   SJ () {
      this.primaryData.SJ = `${this.primaryData.YEARS}年第${this.QUARTER}季度`
      return this.primaryData.SJ
    },
     
   primaryData: {
      get: function () {
        return this.formData.primaryData[0]
      },
      set: function (val) {
        this.formData.primaryData[0] = val
      }
    },
  }	
}
</script>
```


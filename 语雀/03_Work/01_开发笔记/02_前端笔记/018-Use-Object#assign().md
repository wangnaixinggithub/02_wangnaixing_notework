# Use-Object#assign()

# 1、引用值传递

```javascript
<script>
//1.JS 分配新的内存 提供引用指向该空间。赋值给临时的变量 primaryDataFromMe 假设为Ox123
let primaryDataFromMe = {}

//2.JS会创建下 面三个属性的子内存空间, 并赋值。
primaryDataFromMe.ZDZDKDCL_ZAOFENG = 200000
primaryDataFromMe.ZDZDKDCL_WUFENG  = 300000
primaryDataFromMe.ZDZDKDCL_WANFENG = 400000


const calcJhxx = (primaryData, jhxxData = []) => {     
    //primaryData => VUE v-model  假设为Ox456
    
    //3.只是把 primaryDataFromMe 引用给 primaryData   此时 primaryData  primaryDataFromMe 都是 Ox123 
    primaryData = primaryDataFromMe
    
    //4.此时做的改变，不会影响到Vue绑定的对象。 即不会 影响到 Ox456 内存空间存的值
    primaryData.TZXS_ZAOFENG = 1.6707
 }
</script>
```

# 2、`#Object.assign（source,target）`

> 用于对象属性复制，会把旧对象全部属性值复制，在新对象新增属性子内存空间，进行赋值。如果新对象本身存在属性，则覆盖原有值。

```javascript
<script>
    
let primaryDataFromMe = {}
primaryDataFromMe.ZDZDKDCL_ZAOFENG = 200000
primaryDataFromMe.ZDZDKDCL_WUFENG  = 300000
primaryDataFromMe.ZDZDKDCL_WANFENG = 400000

const calcJhxx = (primaryData, jhxxData = []) => {     
 
    // primaryDataFromMe All prop Copy To  primaryData
 
    Object.assign(primaryData, primaryDataFromMe)
    
    primaryData.TZXS_ZAOFENG = 1.6707

}

</script>
```


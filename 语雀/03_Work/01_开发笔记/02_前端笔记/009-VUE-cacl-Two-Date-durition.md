# VUE-cacl-Two-Date-durition



- 1、计算两个时间的实现差。定义validate.js，在里面定义getSC()的方法。

```javascript
const getSC =function (beginTime,endTime) {
    
    let dataSC =new Date(endTime) - new Date(beginTime); //计算出时间差 毫秒
    
    //1.计算出天数
    let days = Math.floor(dataSC/(24*3600*1000)); 
    
    let leave1=dataSC%(24*3600*1000)    //计算天数后剩余的毫秒数
    
    //2.计算出小时
    let hours=Math.floor(leave1/(3600*1000)); 
    
    let sc = days+"天"+hours+"小时";
    return sc;
}
const validateforme = {
    getSC,
}
export default validateforme;
```

- 2、进行测试，假如两个时间字符如下

```html
  <el-button @click="fun01()"></el-button>
```

```javascript
fun01(){
            let sc = validateforme.getSC('2022-03-23 00:00:00','2022-03-24 00:00:00');
            console.log(sc) //1天0小时
        }
```


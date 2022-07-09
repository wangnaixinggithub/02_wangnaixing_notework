# ElementUI-[el-date-picker]-@change-Three-Params

#  1、@change

1、为组件绑定@change事件，当日期组件被点击确定的时候触发checkTime()函数。

```vue
         <el-date-picker
                        id="SQGZJSSJ"
                        v-model="primaryData.SQGZJSSJ"
                        required :disabled="authority.SQGZJSSJ"
                        type="datetime"
                        format="yyyy-MM-dd HH:mm:ss"
                        value-format="yyyy-MM-dd HH:mm:ss"
                        @change="checkTime('SQGZJSSJ',primaryData.SQGZKSSJ,$event)"
                        placeholder="选择日期时间">
         </el-date-picker>
```

2、一般我们要拿到数据模型的值，做校验。

currTimeStr:对应日期组件的ID，beginTime是数据模型主数据值：`primaryData.SQGZKSSJ`。 $event得到的是得到的是当前用户输入的事件字符串。

```javascript
        checkTime(currTimeStr,beginTime,endTime){
            console.log('我被触发了');
        }
```

# 2、$event

1、$event得到的是得到的是当前用户输入的事件字符串。

```vue
 <el-date-picker
          id="TJSJ"
          v-model="primaryData.TJSJ"
          type="datetime"
          @change="checkTimeTJ($event)"
          format="yyyy-MM-dd HH:mm:ss"
          value-format="yyyy-MM-dd HH:mm:ss"
          placeholder="选择日期时间">
 </el-date-picker>
```

2、此时我在日期组件中选择2022年3月19日，点击确定。在方法中接收event。得到日期字符串序列。

```javascript
  checkTimeTJ(event){
    console.log(event)  //2022-03-19 00:00:00
  }
```


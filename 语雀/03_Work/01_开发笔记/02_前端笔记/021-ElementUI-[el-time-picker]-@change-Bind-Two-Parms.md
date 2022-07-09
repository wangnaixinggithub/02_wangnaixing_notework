# ElementUI-[el-time-picker]-@change-Bind-Two-Parms

# bind-Two-Params

> 使用箭头函数,让VUE 时间日期组件el-time-picker的@change()方法绑定两个参数。

```javascript
    el_time_picker_OnChange(val,SJ){
        console.log("我被点击了",val)
        console.log("我被点击了",SJ)
    }
```

```vue
<!--王乃醒添加-->
       <tr>
            <td class="td_title_column">早峰时间<span style="color: red;"> *</span></td>
                             <td class="td_value_column column_top_bottom_right">
                                   <el-time-picker
                                        @change="(value) => el_time_picker_OnChange(value,'ZAOFENGSJ')"
                                        id="ZAOFENGSJ" required
                                        :disabled="!(fieldAuth.zdbz||fieldAuth.zdhz)"
                                        v-model="primaryData.ZAOFENGSJ"
                                        format="HH:mm" value-format="HH:mm"
                                        placeholder="早峰时间">
                                  </el-time-picker>
                              </td>
           
           <td class="td_title_column">午峰时间<span style="color: red;"> *</span></td>
                                <td class="td_value_column column_top_bottom_right">
                                    <el-time-picker
                                        @change="(value) => el_time_picker_OnChange(value,'WUFENGSJ')"
                                        id="WUFENGSJ" required
                                        :disabled="!(fieldAuth.zdbz||fieldAuth.zdhz)"
                                        v-model="primaryData.WUFENGSJ"
                                        format="HH:mm" value-format="HH:mm"
                                        placeholder="午峰时间">
                                    </el-time-picker>
                                </td>
           
         <td class="td_title_column">晚峰时间<span style="color: red;"> *</span></td>
                                <td class="td_value_column column_top_bottom_right">
                                    <el-time-picker
                                        @change="(value) => el_time_picker_OnChange(value,'WANFENGSJ')"
                                        id="WANFENGSJ" required
                                        :disabled="!(fieldAuth.zdbz||fieldAuth.zdhz)"
                                        v-model="primaryData.WANFENGSJ"
                                        format="HH:mm" value-format="HH:mm"
                                        placeholder="晚峰时间">
                                    </el-time-picker>
                                </td>
      </tr>
```

![image-20220519142713870](C:/Users/Administrator.DESKTOP-E0KTJ20/AppData/Roaming/Typora/typora-user-images/image-20220519142713870.png)
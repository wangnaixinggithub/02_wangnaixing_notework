# ElementUI-Validation-Phone-ByRegix

- 1、通过@change来监听el-input输入框内容变化，让他进入指定的`checkPhone()`方法。

```html
<td class="td_value_column column_top_bottom_right">
      <el-input id="LXDH" 
              v-model="primaryData.LXDH"
              :show-word-limit="true" 
              :maxlength="15" 
             @change="checkPhone(primaryData.LXDH,'LXDH')">
     </el-input>
</td>
```

- 2、通过正则表达式来进行校验。

```javascript
 checkPhone(phoneNumber,eleId){
            if((/^1[3|4|5|6|7|8|9][0-9]{9}$/.test(phoneNumber)) || (/^(\d{3,4}-)\d{7,8}$/.test(phoneNumber))){
              
            }else{ 
               
                //1.让el-inpt的systle 加入 border red 1px;
                
                //2.弹出提示框。
                this.$message({type: "error", message: "电话格式不正确！",offset:100});
            }
        }
```


# VUE-validation-Array

> 因为展示的数据要分为四个季度显示，让用户在对应季度下能够填写到合适的输入框值。所以我用了v-if.

# 1、template

```html
<tr class="column_border">
                            <td class="column_border td_title_column">项目</td>
                            <td class="column_border td_title_column">单位</td>
                           
                     		<template v-if="formData[0].JD === '一'">
                                <td class="column_border td_title_column">1月<span style="color: #FF0000">*</span></td>
                                <td class="column_border td_title_column">2月<span style="color: #FF0000">*</span></td>
                                <td class="column_border td_title_column">3月<span style="color: #FF0000">*</span></td>
                                <td class="column_border td_title_column">1~3月<span style="color: #FF0000">*</span></td>
                            </template>
                            
                     		<template v-else-if="formData[0].JD === '二'">
                                <td class="column_border td_title_column">4月<span style="color: #FF0000">*</span></td>
                                <td class="column_border td_title_column">5月<span style="color: #FF0000">*</span></td>
                                <td class="column_border td_title_column">6月<span style="color: #FF0000">*</span></td>
                                <td class="column_border td_title_column">4~6月<span style="color: #FF0000">*</span></td>
                            </template>
                            
                     
                     		<template v-else-if="formData[0].JD === '三'">
                                <td class="column_border td_title_column">7月<span style="color: #FF0000">*</span></td>
                                <td class="column_border td_title_column">8月<span style="color: #FF0000">*</span></td>
                                <td class="column_border td_title_column">9月<span style="color: #FF0000">*</span></td>
                                <td class="column_border td_title_column">7~9月<span style="color: #FF0000">*</span></td>
                            </template>
                            
                     		<template v-else-if="formData[0].JD === '四'">
                                <td class="column_border td_title_column">10月<span style="color: #FF0000">*</span></td>
                                <td class="column_border td_title_column">11月<span style="color: #FF0000">*</span></td>
                                <td class="column_border td_title_column">12月<span style="color: #FF0000">*</span></td>
                                <td class="column_border td_title_column">10~12月<span style="color: #FF0000">*</span>
                                </td>
                            </template>
                           
                     	<td class="column_border td_title_column">水电运行说明<span style="color: #FF0000">*</span></td>
</tr>
```

```html
 <tr class="column_border" 
     v-for="(item,index) in formData" 
     :key="index">
                            
            <td class="column_border td_title_column">{{ item.XM }}<span style="color: #FF0000">*</span></td>
            <td class="column_border td_title_column">{{ item.XMDW }}</td>
                          
     						<template v-if="formData[0].JD === '一'">
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('JANUARY',index)"
                                              v-model.trim="item.JANUARY"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('FEBRUARY',index)"
                                              v-model.trim="item.FEBRUARY"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('MAR',index)" v-model.trim="item.MAR"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('ONE_THREE',index)"
                                              v-model.trim="item.ONE_THREE"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('SDYXSM',index)" v-model="item.SDYXSM"></el-input>
                                </td>
                            </template>
     
                            <template v-else-if="formData[0].JD === '二'">
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('APRIL',index)" v-model.trim="item.APRIL"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('MAY',index)" v-model.trim="item.MAY"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('JUNE',index)" v-model.trim="item.JUNE"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('FOUR_SIX',index)"
                                              v-model.trim="item.FOUR_SIX"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('SDYXSM',index)" v-model="item.SDYXSM"></el-input>
                                </td>
                            </template>
     
     
                            <template v-else-if="formData[0].JD === '三'">
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('JULY',index)" v-model.trim="item.JULY"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('AUGUST',index)" v-model.trim="item.AUGUST"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('SEPTEMBER',index)"
                                              v-model.trim="item.SEPTEMBER"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('SEVEN_NINE',index)"
                                              v-model.trim="item.SEVEN_NINE"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('SDYXSM',index)" v-model="item.SDYXSM"></el-input>
                                </td>
                            </template>
     
     
                            <template v-else-if="formData[0].JD === '四'">
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('OCTOBER',index)"
                                              v-model.trim="item.OCTOBER"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('NOVEMBER',index)"
                                              v-model.trim="item.NOVEMBER"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('DECEMBER',index)"
                                              v-model.trim="item.DECEMBER"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('TEN_TWELVE',index)"
                                              v-model.trim="item.TEN_TWELVE"></el-input>
                                </td>
                                <td class="column_border td_value_column">
                                    <el-input :id="generateNewEleId('SDYXSM',index)" v-model="item.SDYXSM"></el-input>
                                </td>
                    		 </template>
</tr>
```

```javascript
//生成新的ID
generateNewEleId(formDataFiled, index,) {return formDataFiled + index;}
```

# 2、method

```javascript
  btnSave_onclick() {
            //数据校验
            let validateResult = validate.validateArray(this.formData,this.formData[0].JD)
            if (validateResult.flag === false) {
                this.$message({type: "error", message: validateResult.msg, offset: 100});
            } else {
                console.log("正在发起后端请求")

            }

        },
```

#       3、validate.js

```javascript
const SdjjhDetailFormRequiredFieldsByFirstReason = "JANUARY,FEBRUARY,MAR,ONE_THREE,SDYXSM";
const SdjjhDetailFormRequiredFieldsByTwoReason = "APRIL,MAY,JUNE,FOUR_SIX,SDYXSM";
const SdjjhDetailFormRequiredFieldsByThreeReason = "JULY,AUGUST,SEPTEMBER,SEVEN_NINE,SDYXSM";
const SdjjhDetailFormRequiredFieldsByFourReason = "OCTOBER,NOVEMBER,DECEMBER,TEN_TWELVE,SDYXSM";

validateArray:function (formDataArray,JD){
           
        let result = {
                flag: true,
                msg: "校验成功！"
            }
            
            let requiredFieldArray = []

            switch (JD){
                case '一':
                    requiredFieldArray = SdjjhDetailFormRequiredFieldsByFirstReason.split(",")
                    break
                case '二':
                    requiredFieldArray = SdjjhDetailFormRequiredFieldsByTwoReason.split(",")
                    break
                case '三':
                    requiredFieldArray = SdjjhDetailFormRequiredFieldsByThreeReason.split(",")
                    break
                case '四':
                    requiredFieldArray = SdjjhDetailFormRequiredFieldsByFourReason.split(",")
                    break

            }
    
		   	//在外面套一层循环解决。
                for(let i = 0; i< formDataArray.length; i++) {
                    for (let fieldName of requiredFieldArray) {
                    if (!(formDataArray[i])[fieldName] || formDataArray[i].fieldName === '' ){
                        this.setErrorStyleByEleId(`${fieldName}${i}`)
                        result.flag = false;
                        result.msg = "存在必填项未填写，请核对！";
                    }else {
                        this.clearErrorStyleByEleId(`${fieldName}${i}`)
                    }

                }
            }
        return result
    }
```


# Element-UI-[el-Input]-Color-Red

# 1、DomId => V-modelFiled

给输入框组件ID属性，让他和数据库字段保存一一对应关系

```vue
 <el-input id="SQDBH"
           required :disabled="authority.SQDBH"
           v-model="primaryData.SQDBH"
           :show-word-limit="true"
           :maxlength="30"
           readonly>
 </el-input>
```

渲染后：

```html
<input type="text" readonly="readonly" autocomplete="off" id="SQDBH" required="required" maxlength="30" class="el-input__inner">
```

# 2、style provider class

- 创建显示红色边框的类选择器

```css
<style scoped lang="less">
/deep/ .required_error {
    border: 1px solid red;
}
</style>
```

# 3、validate.js

```javascript
const setErrorStyleByEleId = function(eleId){
	 let el = document.getElementById(eleId)
            let elClass =  el.getAttribute("class") || ""
            let elStyle = el.getAttribute("style") || ""
            if (!elClass.includes(" required_error")){
                elClass += " required_error"  
            }
            if (!elStyle.includes("border: 1px solid red;")){
                elStyle = "border: 1px solid red" + elStyle
            }
            el.setAttribute("class",elClass)
            el.setAttribute("style",elStyle)
}

const clearErrorStyleByEleId = function(eleId){
     let el = document.getElementById(eleId)
            let elClass = el.getAttribute("class") || ""
            let elStyle = el.getAttribute("style") || ""
            elClass =  elClass.replace(" required_error","")
            elStyle = elStyle.replace("border: 1px solid red;","")
            el.setAttribute("class",elClass)
            el.setAttribute("style",elStyle)
}

const validate ={
    setErrorStyleByEleId,
    clearErrorStyleByEleId,
}

export default validate;
```


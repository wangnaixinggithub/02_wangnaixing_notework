# Use-JS-Set-Error-Style

# 1、SET  Error Style

```javascript
const clearErrorStyleByEleId = eleId => {
    //1.获取元素
    let el = document.getElementById(eleId);

    //2.清空错误红框样式
    let elClass = el.getAttribute("class") || "";
    let elStyle = el.getAttribute("style") || "";
    elClass = elClass.replace(" required_error","");
    elStyle = elStyle.replace("border: 1px solid red;", "");

    el.setAttribute("class", elClass);
    el.setAttribute("style", elStyle);
}

```

# 2、Clear Error Style

```javascript
const clearErrorStyleByEleId = eleId => {
    //1.获取元素
    let el = document.getElementById(eleId);

    //2.清空错误红框样式
    let elClass = el.getAttribute("class") || "";
    let elStyle = el.getAttribute("style") || "";
    elClass = elClass.replace(" required_error","");
    elStyle = elStyle.replace("border: 1px solid red;", "");

    el.setAttribute("class", elClass);
    el.setAttribute("style", elStyle);
}
```

# 3、Style

```javascript
<style>
    /deep/ .required_error {
        border: 1px solid red;
    }
</style>
```


# JS-DOM-Use-KeyDown-Handle-InputChange

- 通过绑定输入框的聚焦事件，离焦事件，键盘事件，模拟京东的快递单号输入。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style></style>
</head>
<style>
    * {
        margin: 0;
        padding: 0;
    }

    .search {
        position: relative;
        width: 178px;
        margin: 100px;
    }

    .con {
        display: none;
        position: absolute;
        top: -40px;
        width: 171px;
        border: 1px solid rgba(0, 0, 0, .2);
        box-shadow: 0 2px 4px rgba(0, 0, 0, .2);
        padding: 5px 0;
        font-size: 18px;
        line-height: 20px;
        color: #333;
    }

    .con::before {
        content: '';
        width: 0;
        height: 0;
        position: absolute;
        top: 28px;
        left: 18px;
        border: 8px solid #000;
        border-style: solid dashed dashed;
        border-color: #fff transparent transparent;
    }
</style>

<body>
<div class="search">
    <div class="con">123</div>
    <input class="jd" type="text" placeholder="请输入您的快递单号">
</div>

<script>
   let divElement = document.querySelector('.con')
   let inputElement = document.querySelector('.jd')

   //1.监听输入框键盘事件
   inputElement.addEventListener('keydown',() =>{
    this.handleInputChange()

   })

   //2.监听输入框聚焦事件
   inputElement.addEventListener('focus',()=>{
     this.handleInputChange()

   })

   //3.监听输入框离焦事件
   inputElement.addEventListener('blur',()=>{
       divElement.style.display = 'none'

   })

   function handleInputChange(){
       let valueIsNotNull = inputElement.value && inputElement.value !== ''
       
       if (valueIsNotNull){
           divElement.style.display = 'block'
           divElement.innerText = inputElement.value
       }else {
           divElement.style.display = 'none'
       }
   }





</script>
</body>

</html>
```


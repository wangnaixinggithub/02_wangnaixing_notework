# JS-Array-#every()

判断数组中的每一项是否符合条件。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Array#every()</title>
</head>
<body>


<script>
    function Product(id,name,checkStatu){
        this.id = id
        this.name = name
        this.checkStatu =checkStatu
    }
    let product1 = new Product(1,'南京',true)
    let product2 = new Product(2,'真龙',true)
    let product3 = new Product(3,'红塔山',true)

    const productArr = new Array(product1,product2,product3)

    //every() 会判断lambda的条件是否都成立，如果都成立了返回true 如果有一个数组元素 不成立返回false.
    let flag =  productArr.every(item => Object.is(item.checkStatu,true))

    if (flag){
        console.log('当前购物车所有的产品都被选中了')
    }



</script>
</body>
</html>
```


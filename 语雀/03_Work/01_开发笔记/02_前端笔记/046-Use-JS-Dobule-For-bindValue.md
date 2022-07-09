# Use-JS-Double-For-bindValue

- 一个`DFDPC` 有早午晚。构造后端返回数据为key=要赋值的列，value为`DQ`为key，对应值的列。可完成对对象数组指定列赋值绑定。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>17_数据绑定到数组元素的指定列</title>
</head>
<body>
<script>

    //1.后端返回的数据
    let res = {
        data: {
            DFDPC_ZAO:   {
                '广州':4000,
                '深圳':5000,
                '珠海':6000
            },
            DFDPC_WU:   {
                '广州':4100,
                '深圳':4200,
                '珠海':4300
            },

            DFDPC_WAN: {
                '广州':2200,
                '深圳':3200,
                '珠海':4200
            },
        }
    }
    
    //2.需要赋值的VUE数据模型
    function Jhxx(DQ,DFDPC_ZAO,DFDPC_WU,DFDPC_WAN){
        this.DQ = DQ
        this.DFDPC_ZAO = DFDPC_ZAO
        this.DFDPC_WU=  DFDPC_WU
        this.DFDPC_WAN = DFDPC_WAN
    }

    let jhxx01 = new Jhxx('广州',4100,5100,6100)
    let jhxx02 = new Jhxx('深圳',3000,3100,3200)
    let jhxx03 = new Jhxx('珠海',6100,6000,6200)
    let jhData = [jhxx01,jhxx02,jhxx03]

    
    
    
    //3.Your RollBack Function
    
    //1.forEach jhData[] => jh element
    
    //2.每一个jh element 遍历所有的res.data的成员
    
    //3.找到成员下每一个地区对应值，赋值给 jh element DFDPC
    jhData.forEach(obj => {
        for (let key in res.data) {
            obj[key] = res.data[key][obj.DQ]
        }
    })
    
</script>
</body>
</html>
```


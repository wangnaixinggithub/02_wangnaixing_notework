# Use-JS-Object-propertyValue-New RequestParam

- 请求参数构造：我们可以对对象集合的某些属性值构建成一个新的对象。比如下面`地区:地方电基准值`构建object

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>17_拼接请求参数</title>
</head>
<body>
<script>
    
    function Jhxx(DQ,DFDJZZ_ZAO,DFDJZZ_WU,DFDJZZ_WAN){
            this.DQ = DQ
            this.DFDPC_ZAO = DFDJZZ_ZAO
            this.DFDPC_WU=  DFDJZZ_WU
            this.DFDPC_WAN = DFDJZZ_WAN

    }

    let jhxx01 = new Jhxx('广州',4100,5100,6100)
    let jhxx02 = new Jhxx('深圳',3000,3100,3200)
    let jhxx03 = new Jhxx('珠海',6100,6000,6200)
    let jhData = [jhxx01,jhxx02,jhxx03]


    //1.地区key 地方电基准值为value
    let requestParam = {}


    jhData.forEach(obj=>{
        requestParam[obj.DQ] =  obj['DFDPC_WAN']
    })


    console.log(requestParam)

</script>
</body>
</html>
```

- 可以对filedValue进行一个限定，更加灵活。

```javascript
<script>  
el_time_picker_OnChange() {
            let requestParam = [
                {
                    queryTime: this.primaryData.ZAOFENGSJ,
                    bindingFiled: 'DFDPC_ZAO',
                    jhData: this.handleRequestParam('DFDJZZ_ZAO')
                },
                {
                    queryTime: this.primaryData.WUFENGSJ,
                    bindingFiled: 'DFDPC_WU',
                    jhData: this.handleRequestParam('DFDJZZ_WU')
                },
                {
                    queryTime: this.primaryData.WANFENGSJ,
                    bindingFiled: 'DFDPC_WAN',
                    jhData: this.handleRequestParam('DFDJZZ_WAN')
                }
            ]

            
            dataApi.initjhxxByTime(requestParam).then(res => {
             //	请求后端

            })

          

        },
handleRequestParam(filed){
            let param = {}

            this.jhData.forEach(obj => {
                param[obj.DQ] = obj[filed]
            })

            return param
        }
</script>
```


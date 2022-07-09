# JS-DOM-#removeChild()-dynamic-table

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        table {
            width: 500px;
            margin: 100px auto;
            border-collapse: collapse;
            text-align: center;
        }

        td,
        th {
            border: 1px solid #333;
        }

        thead tr {
            height: 40px;
            background-color: #ccc;
        }
    </style>
</head>

<body>
<table cellspacing="0">
    <thead>
    <tr>
        <th>姓名</th>
        <th>科目</th>
        <th>成绩</th>
        <th>操作</th>
    </tr>
    </thead>
    <tbody>

    </tbody>
</table>
<script>

    let datas = [{
        name: '魏璎珞',
        subject: 'JavaScript',
        score: 100
    }, {
        name: '弘历',
        subject: 'JavaScript',
        score: 98
    }, {
        name: '傅恒',
        subject: 'JavaScript',
        score: 99
    }, {
        name: '明玉',
        subject: 'JavaScript',
        score: 88
    }, {
        name: '大猪蹄子',
        subject: 'JavaScript',
        score: 0
    }]

    let tbodyElement = document.querySelector('tbody')

    for (let i = 0; i < datas.length; i++) {
        let trElement = document.createElement('tr')

        //1.添加表格元素
        for (let key in datas[i]) {
           let tdElement = document.createElement('td')
            tdElement.innerText = datas[i][key]
            trElement.append(tdElement)
        }

        //2.添加删除按钮
        let aElement = document.createElement('td')
        aElement.innerHTML = '<a href="javascript:;">删除</a>'

        trElement.appendChild(aElement)
        tbodyElement.appendChild(trElement)

    }

    //删除功能实现
   let aElements =  document.querySelectorAll('a')
    for (let i = 0; i < aElements.length; i++) {
        aElements[i].onclick = () =>{
            tbodyElement.removeChild(aElements[i].parentNode.parentNode)
        }
    }




</script>
</body>

</html>
```


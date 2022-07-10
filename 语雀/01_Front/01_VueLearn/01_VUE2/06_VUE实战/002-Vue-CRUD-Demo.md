# Vue-CRUD-Demo

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
    <script src="https://unpkg.com/dayjs@1.8.21/dayjs.min.js"></script>
    <link rel="stylesheet" href="https://cdn.staticfile.org/twitter-bootstrap/5.1.1/css/bootstrap.min.css">
    <style>
        body {
            padding: 15px;
            user-select: none;
        }
    </style>
</head>
<body>
<div id="app">
    <div class="card">
        <h3 class="card-header" style="text-align: center">添加学生信息</h3>
        <div class="card-body">
            <form @submit.prevent="formSubmitOnAction">
                <div class="mb-3">
                    <label for="stuName" class="form-label">学生姓名</label>
                    <input type="text" class="form-control" v-model="formData.stuName" id="stuName" aria-describedby="stuNameHelp">
                </div>
                <div class="mb-3">
                    <label for="stuClass" class="form-label">学生班级</label>
                    <input type="text" class="form-control" v-model="formData.stuClass" id="stuClass">
                </div>
                <button type="submit" class="btn btn-primary">添加</button>
            </form>
        </div>
    </div>
    <div style="height: 40px"></div>
    <table class="table table-bordered table-hover table-striped">
        <thead>
            <tr>
                <th scope="col">ID</th>
                <th scope="col">状态</th>
                <th scope="col">学生姓名</th>
                <th scope="col">学生班级</th>
                <th scope="col">入学时间</th>
                <th scope="col">操作</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="item in tableData" :key="item.id">
                <td>{{item.id}}</td>
                <td>
                    <div class="form-check form-switch">
                        <input  type="checkbox"  class="form-check-input" role="switch" v-model="item.status">
                    </div>
                </td>
                <td>
                    {{item.stuName}}
                </td>
                <td>
                    {{item.stuClass}}
                </td>
                <td>
                    {{item.entryTime | formatDate}}
                </td>
                <td>
                    <button type="button" class="btn btn-danger " @click="btnRemoveOnClick(item.id)">删除</button>
                </td>

            </tr>

        </tbody>
    </table>

</div>
<script>
    const app = new Vue({
        el:"#app",
        data:{
            formData:{
                stuName:'',
                stuClass:''
            },//表单对象
            nextId:4,//下一个ID
            tableData:[
                {id:1,status:true,stuName:'王乃醒1',stuClass:'18软件工程6班',entryTime:new Date()},
                {id:2,status:true,stuName:'王乃醒2',stuClass:'18软件工程6班',entryTime:new Date()},
                {id:3,status:false,stuName:'王乃醒3',stuClass:'18软件工程6班',entryTime:new Date()},
            ] //表格展示对象

        },
        filters:{
            formatDate(date){
                return dayjs(date.getTime()).format('YYYY-MM-DD')
            }
        },
        methods:{
          formSubmitOnAction(){
             let {stuName,stuClass} = this.formData
             if (Object.is(stuName,'') || Object.is(stuClass,'')) return alert('数据无填写！')

            //1.insert 操作
            this.formData.id  = this.nextId
            this.formData.entryTime  =  new Date()
            this.formData.status = true
            this.tableData.push(this.formData)


            //2.清空模型操作
            this.formData = {}
            this.nextId++
          },
          btnRemoveOnClick(id){
                this.tableData = this.tableData.filter(item => !Object.is(item.id,id))
          }
        }


    })
</script>
</body>
</html>
```


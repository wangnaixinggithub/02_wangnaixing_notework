# Vue-@submit.prevent

阻止表单默认提交行为`@submit.prevent`，让他进入到业务方法`#formSubmitOnAction()`

```html
<div class="card">
        <h3 class="card-header" style="text-align: center">添加学生信息</h3>
        <div class="card-body">
            <form @submit.prevent="formSubmitOnAction">
                <div class="mb-3">
                    <label for="stuName" class="form-label">学生姓名</label>
                    <input type="email" class="form-control" id="stuName" aria-describedby="stuNameHelp">
                </div>
                <div class="mb-3">
                    <label for="stuClass" class="form-label">学生班级</label>
                    <input type="password" class="form-control" id="stuClass">
                </div>
                <button type="submit" class="btn btn-primary">添加</button>
            </form>
        </div>
    </div>
```

```javascript
methods:{
          formSubmitOnAction(){
                console.log('我进来了哈哈哈')
          }
        }
```


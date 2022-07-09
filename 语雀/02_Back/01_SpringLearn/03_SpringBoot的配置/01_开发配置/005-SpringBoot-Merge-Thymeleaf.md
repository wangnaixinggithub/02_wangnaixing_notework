# SpringBoot-Merge-Thymeleaf



## 1、`xmlns:th="http://www.thymeleaf.org">`

```yaml
# application.yaml
spring:   
 thymeleaf:
   prefix: classpath:/templates
   suffix: .html
   encoding: UTF-8
   mode: HTML
   template-resolver-order: 1
   cache: false
```

```xml
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>用户列表</title>
</head>
<body>
<table border="1">
    <thead>
        <tr>
            <td>用户名</td>
            <td>姓名</td>
            <td>性别</td>
        </tr>
    </thead>
    <tbody>
    <tr th:each="item:${userList}">
        <td th:text="${item.getUsername()}" t></td>
        <td th:text="${item.getNickName()}"></td>
        <td th:text="${item.getSex()}"></td>
    </tr>
    </tbody>
</table>
</body>
</html>
```

## 2、JS-Get-Response-Model

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>用户列表</title>
</head>
<body>
<table border="1">
    <thead>
        <tr>
            <td>用户名</td>
            <td>姓名</td>
            <td>性别</td>
        </tr>
    </thead>
    <tbody id="tbody">

    </tbody>
</table>
<script th:inline="javascript">
    let userList = [[${userList}]]
    let element = document.getElementById("tbody")
    let trHtml = ''
    userList.forEach(item=>{
        trHtml += `
        <tr>
               <td>${item.username}</td>
               <td>${item.nickName}</td>
               <td>${item.sex}</td>
        </tr>
        `
    })
   element.innerHTML = trHtml
</script>
</body>
</html>
```

## 3、As-Email-Template

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>邮件模板</title>
</head>
<body>
<p>
    Hello！欢迎<span th:text="${username}"/>
</p>
<p>
    您的职位信息为：<span th:text="${position}"></span>
</p>
<p>
    报到地址：<span th:text="${address}"></span>
</p>
<p>
    入职联系人:<span th:text="${contactMan}"></span>
</p>
<p>
    薪资待遇：<span th:text="${salary}"></span>
</p>
</body>
</html>
```

```java
@Controller
@RequestMapping("/user")
public class UserController {
    @Autowired
    private TemplateEngine templateEngine;
    
    @GetMapping("/sendEmail")
    public void sendEmail(){
        Context context = new Context();
        context.setVariable("username","王乃醒");
        context.setVariable("position","实习生");
        context.setVariable("address","广东省广州市天河区华强路一号珠控国际中心803");
        context.setVariable("contactMan","洪磊");
        context.setVariable("salary","150/天");
        String email = templateEngine.process("email", context);
        //省略邮件发送API调用过程.....
        
    }
 }
```

## 4、Join Query Param

- 以删除学生和根据ID获取学生信息为例子，链接凭接查询参数。

```html
<td>
  <div class="button-group">
    <a class="button border-main" th:href="@{'/student/findById/'+${item.id}}">
      <span class="icon-edit"></span> 修改
    </a>
    <a class="button border-red"
       th:href="@{'/student/delete/'+${item.id}}">
      <span class="icon-trash-o"></span>	删除
    </a>
  </div>
 </td>
```

## 5、`th:value`

    - 以修改学生数据回显为例！

```html
	<div class="body-content">
			<form method="POST" class="form-x" th:action="@{'/student/update'+${student.id}}" >
				<div class="form-group">
					<div class="label">
						<label>学生编号：</label>
					</div>
					<div class="field">
						<input type="text" class="input w50" th:value="${student.stuId}" name="stuId"
							data-validate="required:请输入用户编号" />
						<div class="tips"></div>
					</div>
				</div>
				<div class="form-group">
					<div class="label">
						<label>学生姓名：</label>
					</div>
					<div class="field">
						<input type="text" class="input w50" th:value="${student.stuName}" name="stuName"
							   data-validate="required:请输入用户编号" />
						<div class="tips"></div>
					</div>
				</div>
				<div class="form-group">
					<div class="label">
						<label>性别：</label>
					</div>
					<div class="field">
						<input type="text" class="input w50" th:value="${student.stuSex}"  name="stuSex"
							   data-validate="required:请输入用户编号" />
						<div class="tips"></div>
					</div>
				</div>
				<div class="form-group">
					<div class="label">
						<label>地址：</label>
					</div>
					<div class="field">
						<input type="text" class="input w50" th:value="${student.stuAddress}"  name="stuAddress"
							   data-validate="required:请输入用户编号" />
						<div class="tips"></div>
					</div>
				</div>
				<div class="form-group">
					<div class="label">
						<label>班级ID：</label>
					</div>
					<div class="field">
						<input type="text" class="input w50" th:value="${student.classId}" name="classId"
							   data-validate="required:请输入用户编号" />
						<div class="tips"></div>
					</div>
				</div>
				<div class="form-group">
					<div class="label">
						<label>班级名称：</label>
					</div>
					<div class="field">
						<input type="text" class="input w50" th:value="${student.className}" name="className"
							   data-validate="required:请输入班级姓名" />
						<div class="tips"></div>
					</div>
				</div>

				<div class="form-group">
					<div class="label">
						<label></label>
					</div>
					<div class="field">
						<button class="button bg-main icon-check-square-o" type="submit">
							提交</button>
					</div>
				</div>
			</form>
		</div>
```

## 6、`th:class  th:if`

- 以分页导航为例，根据条件显示展示内容以及样式。

```html
  <tr>
                <td colspan="8">
                    <div class="pagelist">
                        <a th:if="${page.getPageNum()>0}" th:href="@{'/student/findByPage?pageNo='+${page.getPageNum()-1}}">上一页</a>
                        <a th:class="${item != page.getPageNum()+1}?'':current"
                           th:each="item:${#numbers.sequence(1,page.getTotalPage())}"
                           th:href="@{'/student/findByPage?pageNo='+${item}}"
                           th:text="${item}">
                        </a>
                        <a th:if="${page.getPageNum()<page.getTotalPage()-1}" th:href="@{'/student/findByPage?pageNo='+${page.getPageNum()+1}}">下一页</a>
                    </div>
                </td>
            </tr>
```

## 7、#dates.format()

```html
          <div class="layui-form-item">
                        <label class="layui-form-label required">入职时间</label>
                        <div class="layui-input-block">
                            <input type="text" name="entryDate" th:value="*{#dates.format(user.getCreateTime(),'yyyy-MM-dd')}" id="date" lay-verify="date" placeholder="请选择入职时间"
                                   autocomplete="off" class="layui-input">
                        </div>
         </div>
```


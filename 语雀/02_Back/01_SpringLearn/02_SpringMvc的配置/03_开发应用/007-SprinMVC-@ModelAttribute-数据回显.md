# SprinMVC-@ModelAttribute-数据回显

> 实现修改时候，表单输入数据回显。

## 1、方式一手动添加。

```java
@Controller
public class BeanValidationController {
	
    @GetMapping("/addStudent")
    public ModelAndView addStudent(){
        ModelAndView mv = new ModelAndView("addStudent");
        //存好这个变量student  //在请求域中存key为student values = new Student()
        mv.addObject("student",new Student());
        return mv;
    }

    @PostMapping("/addStudent")
    public String handleFormSubmit(@Validated(ValidationGroup2.class)  Student student, BindingResult bindingResult){
            if (bindingResult != null){
                List<ObjectError> allErrors = bindingResult.getAllErrors();
                allErrors.forEach(error->{
                    System.out.println(error.getObjectName()+":"+error.getDefaultMessage());
                });
                return "addStudent";
            }
            return "success";
    }
}
```

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>添加学生页面</title>
</head>
<body>
<form th:action="@{/addStudent}" method="post"  >
    学生编号： <input type="text" name="id" th:value="${student.id}"> <br/>
    学生姓名： <input type="text" name="name" th:value="${student.name}">  <br/>
    学生年龄： <input type="text" name="age" th:value="${student.age}"> <br/>
    学生邮件： <input type="text" name="email" th:value="${student.email}"> <br/>
    <button type="submit">提交</button>
</form>
</body>
</html>
```

## 2、使用@ModelAttribute

```java

@Controller
public class BeanValidationController {

    @ModelAttribute("student")
    public Student student(){
        return new Student();
    }

    @GetMapping("/addStudent")
    public ModelAndView addStudent(){
        ModelAndView mv = new ModelAndView("addStudent");
        return mv;
    }

    @PostMapping("/addStudent")
    public String handleFormSubmit(@Validated(ValidationGroup2.class) Student student, BindingResult bindingResult){
            if (bindingResult != null){
                List<ObjectError> allErrors = bindingResult.getAllErrors();
                allErrors.forEach(error->{
                    System.out.println(error.getObjectName()+":"+error.getDefaultMessage());
                });
                return "addStudent";
            }
            return "success";
    }



}

```


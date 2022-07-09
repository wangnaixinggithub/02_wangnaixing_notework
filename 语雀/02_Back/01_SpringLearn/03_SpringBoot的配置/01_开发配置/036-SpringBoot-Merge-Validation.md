

# SpringBoot-Merge-Validation

# 1、 Need Config

```xml
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

```java
@Data
public class ValidVO implements Serializable {
   
    /**
     * ID
     */
    private String id;
     
     /**
     * 应用ID
     */
    @Length(min = 6,max = 12,message = "appId长度必须位于6到12之间")
    private String appId;
	
    /**
     * 名字
     */
    @NotBlank(message = "名字为必填项")
    private String name;
	
    
    /**
     * 邮箱
     */
    @Email(message = "请填写正确的邮箱地址")
    private String email;
	
    
    /**
     * 性别
     */
    private String sex;
	
    
    /**
     * 级别
     */
    @NotEmpty(message = "级别不能为空")
    private String level;
}

```

常见的约束注解如下：

![](D:\my_notebook\SpringBoot 如何进行参数校验？老鸟们都是这么玩的！\QQ截图20211119104307.png)

# 2、Your Business

```java
@Validated
@RestController
@Slf4j
public class ValidController {

    /**
     * RequestBody校验
     * @param validVO
     * @return
     */
    @PostMapping("/valid/test1")
    public String test1(@Validated @RequestBody ValidVO validVO){
        log.info("validEntity is {}", validVO);
        return "test1 valid success";
    }

    /**
     * Form校验
     * @param validVO
     * @return
     */
    @PostMapping(value = "/valid/test2")
    public String test2(@Validated ValidVO validVO){
        log.info("validEntity is {}", validVO);
        return "test2 valid success";
    }

    /***
     * 单参数校验
     * @param email
     * @return
     */
    @ApiOperation("单参数校验")
    @PostMapping(value = "/valid/test3")
    public String test3(@Email String email){
        log.info("email is {}", email);
        return "email valid success";
    }

    

}

```

这里我们先定义三个方法`test1()`，`test2()`，`test3()`，`test1()`使用了`@RequestBody`注解，用于接受前端发送的`Json`数据，`test2()`模拟表单提交，`test3()`模拟单参数提交。

**注意，当使用单参数校验时需要在Controller上加上`@Validated`注解，否则不生效**。



- 调用`test1()`方法，提示的是`org.springframework.web.bind.MethodArgumentNotValidException`异常

```json
POST http://localhost:8080/valid/test1
Content-Type: application/json

{
  "id": 1,
  "level": "12",
  "email": "47693899",
  "appId": "ab1c"
}
```

```json
{
  "status": 500,
  "message": "Validation failed for argument [0] in public java.lang.String com.jianzh5.blog.valid.ValidController.test1(com.jianzh5.blog.valid.ValidVO) with 3 errors: [Field error in object 'validVO' on field 'email': rejected value [47693899]; codes [Email.validVO.email,Email.email,Email.java.lang.String,Email]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [validVO.email,email]; arguments []; default message [email],[Ljavax.validation.constraints.Pattern$Flag;@26139123,.*]; default message [不是一个合法的电子邮件地址]]...",
  "data": null,
  "timestamp": 1628239624332
}
```

- 调用test2方法，提示的是`org.springframework.validation.BindException`异常

```json
POST http://localhost:8080/valid/test2
Content-Type: application/x-www-form-urlencoded

id=1&level=12&email=476938977&appId=ab1c
```

```json

{
    "status": 500,
    "message": "org.springframework.validation.BeanPropertyBindingResult: 3 errors\nField error in object 'validVO' on field 'email': rejected value [476938977]; codes [Email.validVO.email,Email.email,Email.java.lang.String,Email]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [validVO.email,email]; arguments []; default message [email],[Ljavax.validation.constraints.Pattern$Flag;@6a9f5a61,.*]; default message [请填写正确的邮箱地址]\nField error in object 'validVO' on field 'name': rejected value [null]; codes [NotBlank.validVO.name,NotBlank.name,NotBlank.java.lang.String,NotBlank]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [validVO.name,name]; arguments []; default message [name]]; default message [名字为必填项]\nField error in object 'validVO' on field 'appId': rejected value [ab1c]; codes [Length.validVO.appId,Length.appId,Length.java.lang.String,Length]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [validVO.appId,appId]; arguments []; default message [appId],12,6]; default message [appId长度必须位于6到12之间]",
    "data": null,
    "timestamp": 1637291979508
}
```

- 调用test3方法，提示的是`javax.validation.ConstraintViolationException`异常

```json
POST http://localhost:8080/valid/test3
Content-Type: application/x-www-form-urlencoded

email=476938977
```

```json
{
    "status": 500,
    "message": "test3.email: 不是一个合法的电子邮件地址",
    "data": null,
    "timestamp": 1637292093617
}
```

通过加入`Validator`校验框架可以帮助我们自动实现参数的校验。

# 3、GlobalExceptionHandler

虽然我们之前定义了全局异常拦截器，也看到了拦截器确实生效了，

但是`Validator`校验框架返回的错误提示太臃肿了，不便于阅读，为了方便前端提示，我们需要将其简化一下。

直接修改之前定义的`RestExceptionHandler`，单独拦截参数校验的三个异常：`javax.validation.ConstraintViolationException`，`org.springframework.validation.BindException`，`org.springframework.web.bind.MethodArgumentNotValidException`，代码如下：

```json
 @ExceptionHandler(value = {BindException.class, ValidationException.class, MethodArgumentNotValidException.class})
    public ResponseEntity<ResultData<String>> handleValidatedException(Exception e) {
        ResultData<String> resp = null;

        if (e instanceof MethodArgumentNotValidException) {
            // BeanValidation exception
            MethodArgumentNotValidException ex = (MethodArgumentNotValidException) e;

            resp = ResultData.fail(HttpStatus.BAD_REQUEST.value(),
                    ex.getBindingResult().getAllErrors().stream()
                            .map(ObjectError::getDefaultMessage)
                            .collect(Collectors.joining("; "))
            );
        } else if (e instanceof ConstraintViolationException) {
            // BeanValidation GET simple param
            ConstraintViolationException ex = (ConstraintViolationException) e;
            resp = ResultData.fail(HttpStatus.BAD_REQUEST.value(),
                    ex.getConstraintViolations().stream()
                            .map(ConstraintViolation::getMessage)
                            .collect(Collectors.joining("; "))
            );
        } else if (e instanceof BindException) {
            // BeanValidation GET object param
            BindException ex = (BindException) e;
            resp = ResultData.fail(HttpStatus.BAD_REQUEST.value(),
                    ex.getAllErrors().stream()
                            .map(ObjectError::getDefaultMessage)
                            .collect(Collectors.joining("; "))
            );
        }

        log.error("参数校验异常:{}",resp.getMessage());
        return new ResponseEntity<>(resp,HttpStatus.BAD_REQUEST);
    }
```

```json
POST http://localhost:8080/valid/test1
Content-Type: application/json

{
  "id": 1,
  "level": "12",
  "email": "47693899",
  "appId": "ab1c"
}
```

```json
{
  "status": 400,
  "message": "名字为必填项; 不是一个合法的电子邮件地址; appId长度必须位于6到12之间",
  "data": null,
  "timestamp": 1628435116680
}
```

是不是感觉清爽多了？

虽然`Spring Validation` 提供的注解基本上够用，但是面对复杂的定义，我们还是需要自己定义相关注解来实现自动校验。

比如上面实体类中的sex性别属性，只允许前端传递传 M，F 这2个枚举值，如何实现呢？

# 4、New Your ValidationAnno

- 自定义注解 

```java
@Target({METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER, TYPE_USE})
@Retention(RUNTIME)
@Repeatable(EnumString.List.class)
@Documented
@Constraint(validatedBy = EnumStringValidator.class) //标明由哪个类执行校验逻辑
public @interface EnumString {

    String message() default "value not in enum values.";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};


    String[] value();

    
    @Target({METHOD, FIELD, ANNOTATION_TYPE, CONSTRUCTOR, PARAMETER, TYPE_USE})
    @Retention(RUNTIME)
    @Documented
    @interface List {

        EnumString[] value();
    }
    
}

```

- 自定义基于此注解的校验器

```java
public class EnumStringValidator implements ConstraintValidator<EnumString, String> {
    private List<String> enumStringList;

    @Override
    public void initialize(EnumString constraintAnnotation) {
        enumStringList = Arrays.asList(constraintAnnotation.value());
    }

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        if(value == null){
            return true;
        }
        return enumStringList.contains(value);
    }
}


```

- Use 

```java

    @EnumString(value = {"F","M"}, message="性别只允许为F或M")
    private String sex;
```

```json
{
  "status": 400,
  "message": "性别只允许为F或M",
  "data": null,
  "timestamp": 1628435243723
}
```

# 5、ValidGroup

- 分组校验接口

```java
package com.wnx.blog.valid;

import javax.validation.groups.Default;
/**
 * 分组校验接口
 * @author jam
 * @date 2021/7/30 1:54 下午
 */
public interface ValidGroup extends Default {
       
        interface Crud extends ValidGroup{}
    
        interface Create extends Crud{ }
    
        interface Update extends Crud{ }
    
        interface Query extends Crud{ }
    
        interface Delete extends Crud{ }
    
}

```

这里我们定义一个分组接口`ValidGroup`让其继承`javax.validation.groups.Default`，再在分组接口中定义出多个不同的操作类型，`Create`，`Update`，`Query`，`Delete`。至于为什么需要继承`Default`我们稍后再说。

- 应用不同的校验组

```java
@Data
public class ValidVO implements Serializable {

    /**
     * ID
     */
    @NotNull(groups = ValidGroup.Crud.Update.class,message = "ID不能为空")
    private String id;

    /**
     * 应用ID
     */
    @Length(min = 6,max = 12,message = "appId长度必须位于6到12之间")
    @NotNull(groups = ValidGroup.Crud.Update.class,message = "应用ID不能为空")
    private String appId;

    /**
     * 名字
     */
    @NotBlank(message = "名字为必填项")
    @NotNull(groups = ValidGroup.Crud.Create.class,message = "名字为必填项")
    private String name;

    /**
     * 邮箱
     */
    @Email(message = "请填写正确的邮箱地址")
    private String email;

    /**
     * 性别
     */
    @EnumString(value = {"F","M"}, message="性别只允许为F或M")
    private String sex;

    /**
     * 级别
     */
    @NotEmpty(message = "级别不能为空")
    private String level;
}
```

这里我们通过`value`属性给`add()`和`update()`方法分别指定Create和Update分组。

```java

@RestController
@Slf4j
@Validated
public class ValidController {

    @ApiOperation("新增")
    @PostMapping(value = "/valid/add")
    public String add(@Validated(value = ValidGroup.Crud.Create.class) ValidVO validVO){
        log.info("validEntity is {}", validVO);
        return "test3 valid success";
    }


    @ApiOperation("更新")
    @PostMapping(value = "/valid/update")
    public String update(@Validated(value = ValidGroup.Crud.Update.class) ValidVO validVO){
        log.info("validEntity is {}", validVO);
        return "test4 valid success";
    }
}
```

```json
POST http://localhost:8080/valid/add
Content-Type: application/x-www-form-urlencoded

name=javadaily&level=12&email=476938977@qq.com&sex=F
```

在Create时我们没有传递id和appId参数，校验通过。

当我们使用同样的参数调用update方法时则提示参数校验错误。

```json
{
  "status": 400,
  "message": "ID不能为空; 应用ID不能为空",
  "data": null,
  "timestamp": 1628492514313
}
```

由于email属于默认分组，而我们的分组接口`ValidGroup`已经继承了`Default`分组，所以也是可以对email字段作参数校验的。如：

```json
POST http://localhost:8080/valid/add
Content-Type: application/x-www-form-urlencoded

name=javadaily&level=12&email=476938977&sex=F
```

```json
{
  "status": 400,
  "message": "请填写正取的邮箱地址",
  "data": null,
  "timestamp": 1628492637305
}
```

当然如果你的`ValidGroup`没有继承Default分组，那在代码属性上就需要加上`@Validated(value = {ValidGroup.Crud.Create.class, Default.class})`才能让`email`字段的校验生效。


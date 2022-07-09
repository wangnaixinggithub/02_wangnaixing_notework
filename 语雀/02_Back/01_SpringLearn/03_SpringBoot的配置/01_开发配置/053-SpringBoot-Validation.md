# SpringBoot-Validation



# 1、Need Config

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

- 自定义校验注解

```java

@Documented@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD,ElementType.PARAMETER})
@Constraint(validatedBy = FlagValidatorClass.class)
public @interface FlagValidator {
    String[] value() default {};

    String message() default "flag is not found";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};
}

```

- 自定义校验逻辑实现类,`FlagValidatorClass` 输入的值等不等于0或者1

```java
public class FlagValidatorClass implements ConstraintValidator<FlagValidator,Integer> {
    private String[] values;
    @Override
    public void initialize(FlagValidator flagValidator) {
        this.values = flagValidator.value();
    }

    @Override
    public boolean isValid(Integer value, ConstraintValidatorContext constraintValidatorContext) {
        boolean isValid = false;
        if(value==null){
            //当状态为空时使用默认值
            return true;
        }
        
        for(int i=0;i<values.length;i++){
            if(values[i].equals(String.valueOf(value))){
                isValid = true;
                break;
            }
        }
        
        return isValid;
    }
}

```

- 指定全局异常处理

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ResponseStatus(HttpStatus.BAD_REQUEST)
    @ExceptionHandler(value = MethodArgumentNotValidException.class)
    public CommonResult handleValidException(MethodArgumentNotValidException e) {
        BindingResult bindingResult = e.getBindingResult();
        String message = null;
        if (bindingResult.hasErrors()) {
            FieldError fieldError = bindingResult.getFieldError();
            if (fieldError != null) {
                message = fieldError.getField()+fieldError.getDefaultMessage();
            }
        }
        return CommonResult.failed(message);
    }

    @ResponseStatus(HttpStatus.BAD_REQUEST)
    @ExceptionHandler(value = BindException.class)
    public CommonResult handleValidException(BindException e) {
        BindingResult bindingResult = e.getBindingResult();
        String message = null;
        if (bindingResult.hasErrors()) {
            FieldError fieldError = bindingResult.getFieldError();
            if (fieldError != null) {
                message = fieldError.getField()+fieldError.getDefaultMessage();
            }
        }
        return CommonResult.failed(message);
    }

    @ResponseStatus(HttpStatus.BAD_REQUEST)
    @ExceptionHandler(value = ApiException.class)
    public CommonResult handle(ApiException e) {
        if (e.getErrorCode() != null) {
            return CommonResult.failed(e.getErrorCode());
        }
        return CommonResult.failed(e.getMessage());
    }
}

```

- 自定义异常,有的时候我们想抛出一些自己业务异常，来告知用户。

```java

public class ApiException extends RuntimeException {
    private IErrorCode errorCode;

    public ApiException(IErrorCode errorCode) {
        super(errorCode.getMessage());
        this.errorCode = errorCode;
    }

    public ApiException(String message) {
        super(message);
    }

    public ApiException(Throwable cause) {
        super(cause);
    }

    public ApiException(String message, Throwable cause) {
        super(message, cause);
    }

    public IErrorCode getErrorCode() {
        return errorCode;
    }
}

```

- 用断言的方式，调用异常，提高代码可读性。

```java
public class Asserts {
    public static void fail(String message) {
        throw new ApiException(message);
    }

    public static void fail(IErrorCode errorCode) {
        throw new ApiException(errorCode);
    }
}

```



# 2、Use It

- 实体类加校验规则注解。

```java
@Data
public class PmsBrand implements Serializable {
    private Long id;

    @NotBlank(message = "品牌名称不能为空")
    private String name;

    @ApiModelProperty(value = "首字母")
    @NotBlank(message = "首字母不能为空")
    private String firstLetter;
    
    @NotNull(message = "排序号不能为空")
    private Integer sort;
    
    @ApiModelProperty(value = "是否为品牌制造商：0->不是；1->是")
    @NotNull(message = "是否为品牌制造商不能为空")
    @FlagValidator(value = {"0","1"}, message = "显示状态不正确")
    private Integer factoryStatus;
    
    @NotNull(message = "展示状态不能为空")
    private Integer showStatus;
    
    @NotNull(message = "产品数量不能为空")
    @ApiModelProperty(value = "产品数量")
    private Integer productCount;

    @ApiModelProperty(value = "产品评论数量")
    @NotNull(message = "产品评论数量不能为空")
    private Integer productCommentCount;

    @ApiModelProperty(value = "品牌logo")
    @NotBlank(message = "品牌logo不能为空")
    private String logo;

    @ApiModelProperty(value = "专区大图")
    @NotBlank(message = "专区大图不能为空")
    private String bigPic;

    @ApiModelProperty(value = "品牌故事")
    @NotBlank(message = "品牌故事不能为空")
    private String brandStory;

}
```

- `Mvc`入参方法加校验执行注解。

```java
    @ApiOperation("添加品牌")
    @RequestMapping(value = "/create", method = RequestMethod.POST)
    @ResponseBody
    public CommonResult createBrand(@Valid @RequestBody PmsBrand pmsBrand) {
        return CommonResult.success("");
    }

```

- 抛出自定义业务异常

```java
    @ApiOperation("添加品牌")
    @RequestMapping(value = "/create", method = RequestMethod.POST)
    @ResponseBody
    public CommonResult createBrand(@Valid @RequestBody PmsBrand pmsBrand) {
        Asserts.fail("我要抛出一个异常！！！");
        return CommonResult.success("");
        return commonResult;
    }
```


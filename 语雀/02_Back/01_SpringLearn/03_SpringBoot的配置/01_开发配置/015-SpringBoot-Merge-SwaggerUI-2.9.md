# SpringBoot-Merge-SwaggerUI

```xml
       <!--pom.xml--> 
       <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
```

# 1、SwaggerConfig

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2).pathMapping("/")
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.wnx.boot.controller")).paths(PathSelectors.any())
                .build()
                .apiInfo(
                        new ApiInfoBuilder()
                        .title("王乃醒帅气的SwaggerUI工程啊")
                        .description("王乃醒帅气的SwaggerUI工程的详细信息")
                        .contact(new Contact("王乃醒","URL","827379239@QQ.com"))
                        .version("oms1")
                        .license("new Apache License")
                        .licenseUrl("licenseUrl...")
                        .build()

                );
        
    }
}
```

# 2、@APiModel @ApiModelProperty

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@ApiModel
public class User implements Serializable {
    @ApiModelProperty("ID")
    private Long id;
    @ApiModelProperty("用户名称")
    private String username;
    @ApiModelProperty("姓名")
    private String nickName;
}
```

# 3、@APi @ApiOperation @ApiImplicitParams @ApiImplicitParam

```java
@RestController
@Api(tags = "用户管理接口")
@RequestMapping("/user")
public class UserController {

    @GetMapping("/list")
    @ApiOperation("用户分页列表")
    @ApiImplicitParams({
            @ApiImplicitParam(name = "当前页",value = "pageNo"),
            @ApiImplicitParam(name = "每页大小",value = "pageSize")
    })
    public String list(@RequestParam(value = "pageNo",defaultValue = "1") Integer pageNo,
                       @RequestParam(value = "pageSize",defaultValue = "5") Integer pageSize){
        return "用户列表";
    }

    @GetMapping("/{id}")
    @ApiOperation("根据ID获取用户信息")
    @ApiImplicitParam(name = "ID",value = "id")
    public String getUser(@PathVariable Integer id){
        return "getUser...";
    }

    @PostMapping
    @ApiOperation("添加用户信息")
    @ApiImplicitParam(name = "ID",value = "id")
    public String addUser(@RequestBody User user){
        return "addUser...";
    }

    @DeleteMapping("/{id}")
    @ApiOperation("删除用户信息")
    @ApiImplicitParam(name = "ID",value = "id")
    public String deleteUser(@PathVariable Integer id){
        return "deleteUser...";
    }

    @PutMapping("/{id}")
    @ApiOperation("更新户信息")
    @ApiImplicitParam(name = "ID",value = "id")
    public String updateUser(@PathVariable Integer id){
        return "updateUser...";
    }
}
```

```properties
接口地址:http://localhost:8080/swagger-ui.html#/
```




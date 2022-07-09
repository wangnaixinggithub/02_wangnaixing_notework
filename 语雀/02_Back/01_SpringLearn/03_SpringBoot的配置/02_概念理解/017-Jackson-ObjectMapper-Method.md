# Use-Jackson-ObjectMapper-Method

> import `com.fasterxml.jackson.databind.ObjectMapper`

# 1、`#writeValueAsString()`

- POJO  => JsonStr

```java
@Autowired
ObjectMapper mapper;

@RequestMapping("serialization")
@ResponseBody
public String serialization() {
    try {
        
        User user = new User();
        user.setUserName("mrbird");
        user.setBirthday(new Date());
        String str = mapper.writeValueAsString(user);
        return str;
    
    } catch (Exception e) {
        e.printStackTrace();
    }
    
    return null;
}
```

- 在SpringBoot中，用`@ResponseBody`，把数据以JsonStr字符串的形式写入响应报文中。

```java
@RequestMapping("serialization")
@ResponseBody
public String serialization() {
  
        User user = new User();
        user.setUserName("mrbird");
        user.setBirthday(new Date());
        String str = mapper.writeValueAsString(user);
        return user;
    
}
```

# 2、`#readTree()`

 解析得到`JsonNode`作为根节点，你可以像操作XML DOM那样操作遍历`JsonNode`以获取数据。

```java
String json = "{\"name\":\"mrbird\",\"hobby\":{\"first\":\"sleep\",\"second\":\"eat\"}}";;
JsonNode node = this.mapper.readTree(json);

JsonNode hobby = node.get("hobby");

String first = hobby.get("first").asText();

```

# 3、#readValue()

- JsonStr => POJO

```java

@Autowired
ObjectMapper mapper;

@RequestMapping("readjsonasobject")
@ResponseBody
public String readJsonAsObject() {
    try {
       
        String json = "{\"name\":\"mrbird\",\"age\":26}";
        User user = mapper.readValue(json, User.class);
       
        String name = user.getUserName();
        int age = user.getAge();
        return name + " " + age;
        
    } catch (Exception e) {
       
        e.printStackTrace();
    
    }
    
    return null;
```

- 在SpringBoot中，`@RequestBody`将提交的JSON自动映射到方法参数上。

```java
@RequestMapping("updateuser")
@ResponseBody
public int updateUser(@RequestBody List<User> list){
    return list.size();
}
```

```json
[{"userName":"mrbird","age":26},{"userName":"scott","age":27}]
```

- Spring Boot 能自动识别出List对象包含的是User类，因为在方法中定义的泛型的类型会被保留在字节码中，所以Spring Boot能识别List包含的泛型类型从而能正确反序列化。

有些情况下，集合对象并`没有包含泛型定义`，如下代码所示，反序列化并不能得到期望的结果。

```java
@Autowired
ObjectMapper mapper;

@RequestMapping("customize")
@ResponseBody
public String customize() throws JsonParseException, JsonMappingException, IOException {
    String jsonStr = "[{\"userName\":\"mrbird\",\"age\":26},{\"userName\":\"scott\",\"age\":27}]";
    List<User> list = mapper.readValue(jsonStr, List.class);
    String msg = "";
    for (User user : list) {
        msg += user.getUserName();
    }
    return msg;
}
```

访问`customize`，控制台抛出异常：

```c++
java.lang.ClassCastException: java.util.LinkedHashMap cannot be cast to com.example.pojo.User
```

这是因为在运行时刻，泛型己经被擦除了（不同于方法参数定义的泛型，不会被擦除）。为了提供泛型信息，Jackson提供了`JavaType` ，用来指明集合类型，将上述方法改为：

# 4、JavaType

```java
@Autowired
ObjectMapper mapper;

@RequestMapping("customize")
@ResponseBody
public String customize() throws JsonParseException, JsonMappingException, IOException {
    String jsonStr = "[{\"userName\":\"mrbird\",\"age\":26},{\"userName\":\"scott\",\"age\":27}]";
    JavaType type = mapper.getTypeFactory().constructParametricType(List.class, User.class);
    List<User> list = mapper.readValue(jsonStr, type);
    String msg = "";
    for (User user : list) {
        msg += user.getUserName();
    }
    return msg;
}
```

访问`customize`，页面输出：`mrbirdscott`。
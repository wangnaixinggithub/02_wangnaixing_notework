# Use-Jackson-Anno

# 1、@JsonProperty

`@JsonProperty`，作用在属性上，用来为JSON Key指定一个别名。

```java
@JsonProperty("bth")
private Date birthday;
```

再次访问`getuser`页面输出：

```
{"userName":"mrbird","age":0,"password":null,"bth":"2018-04-02 10:38:37"}
```

key birthday已经被替换为了bth。

# 2、@Jsonlgnore

`@Jsonlgnore`，作用在属性上，用来忽略此属性。

```java
@JsonIgnore
private String password;
```

再次访问`getuser`页面输出：

```
userName":"mrbird","age":0,"bth":"2018-04-02 10:40:45"}
```

password属性已被忽略。

# 3、@JsonIgnoreProperties

`@JsonIgnoreProperties`，忽略一组属性，作用于类上，比如`JsonIgnoreProperties({ "password", "age" })`。

```java
@JsonIgnoreProperties({ "password", "age" })
public class User implements Serializable {
    ...
}
```

再次访问`getuser`页面输出：

```java
{"userName":"mrbird","bth":"2018-04-02 10:45:34"}
```

# 4、@JsonFormat

`@JsonFormat`，用于日期格式化，如：

```java
@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
private Date birthday;
```

# 5、@JsonNaming

`@JsonNaming`，用于指定一个命名策略，作用于类或者属性上。Jackson自带了多种命名策略，你可以实现自己的命名策略，比如输出的key 由Java命名方式转为下面线命名方法 —— userName转化为user-name。

```java
@JsonNaming(PropertyNamingStrategies.SnakeCaseStrategy.class)
public class User implements Serializable {
    ...
}
```

再次访问`getuser`页面输出：

```java
{"user_name":"mrbird","bth":"2018-04-02 10:52:12"}
```

# 6、@JsonSerialize

`@JsonSerialize`，指定一个实现类来自定义序列化。类必须实现`JsonSerializer`接口，代码如下：

```java
import java.io.IOException;

import com.example.pojo.User;
import com.fasterxml.jackson.core.JsonGenerator;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.JsonSerializer;
import com.fasterxml.jackson.databind.SerializerProvider;

public class UserSerializer extends JsonSerializer<User> {

    @Override
    public void serialize(User user, JsonGenerator generator, SerializerProvider provider)
            throws IOException, JsonProcessingException {
        generator.writeStartObject();
        generator.writeStringField("user-name", user.getUserName());
        generator.writeEndObject();
    }
}
```

上面的代码中我们仅仅序列化userName属性，且输出的key是`user-name`。 使用注解`@JsonSerialize`来指定User对象的序列化方式：

```java
@JsonSerialize(using = UserSerializer.class)
public class User implements Serializable {
    ...
}
```

再次访问`getuser`页面输出：

```java
{"user-name":"mrbird"}
```

# 7、@JsonDeserialize

`@JsonDeserialize`，用户自定义反序列化，同`@JsonSerialize` ，类需要实现`JsonDeserializer`接口。

```java
import java.io.IOException;

import com.example.pojo.User;
import com.fasterxml.jackson.core.JsonParser;
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.DeserializationContext;
import com.fasterxml.jackson.databind.JsonDeserializer;
import com.fasterxml.jackson.databind.JsonNode;

public class UserDeserializer extends JsonDeserializer<User> {

    @Override
    public User deserialize(JsonParser parser, DeserializationContext context)
            throws IOException, JsonProcessingException {
        JsonNode node = parser.getCodec().readTree(parser);
        String userName = node.get("user-name").asText();
        User user = new User();
        user.setUserName(userName);
        return user;
    }
}
```

使用注解`@JsonDeserialize`来指定User对象的序列化方式：

```java
@JsonDeserialize (using = UserDeserializer.class)
public class User implements Serializable {
    ...
}
```

​	

```java
@Autowired
ObjectMapper mapper;

@RequestMapping("readjsonasobject")
@ResponseBody
public String readJsonAsObject() {
    try {
        String json = "{\"user-name\":\"mrbird\"}";
        User user = mapper.readValue(json, User.class);
        String name = user.getUserName();
        return name;
    } catch (Exception e) {
        e.printStackTrace();
    }
    return null;
}
```

访问`readjsonasobject`，页面输出：

```
mrbird
```

# 8、JsonView

`@JsonView`，作用在类或者属性上，用来定义一个序列化组。 比如对于User对象，某些情况下只返回userName属性就行，而某些情况下需要返回全部属性。 因此User对象可以定义成如下：

```java
public class User implements Serializable {
    private static final long serialVersionUID = 6222176558369919436L;
    
    public interface UserNameView {};
    public interface AllUserFieldView extends UserNameView {};
    
    @JsonView(UserNameView.class)
    private String userName;
    
    @JsonView(AllUserFieldView.class)
    private int age;
    
    @JsonView(AllUserFieldView.class)
    private String password;
    
    @JsonView(AllUserFieldView.class)
    private Date birthday;
    ...	
}
```

User定义了两个接口类，一个为`userNameView`，另外一个为`AllUserFieldView`继承了`userNameView`接口。这两个接口代表了两个序列化组的名称。属性userName使用了`@JsonView(UserNameView.class)`，而剩下属性使用了`@JsonView(AllUserFieldView.class)`。

Spring中Controller方法允许使用`@JsonView`指定一个组名，被序列化的对象只有在这个组的属性才会被序列化，代码如下：

```java
@JsonView(User.UserNameView.class)
@RequestMapping("getuser")
@ResponseBody
public User getUser() {
    User user = new User();
    user.setUserName("mrbird");
    user.setAge(26);
    user.setPassword("123456");
    user.setBirthday(new Date());
    return user;
}
```

访问`getuser`页面输出：

```json
{"userName":"mrbird"}
```

如果将`@JsonView(User.UserNameView.class)`替换为`@JsonView(User.AllUserFieldView.class)`，输出：

```json
{"userName":"mrbird","age":26,"password":"123456","birthday":"2018-04-02 11:24:00"}
```

因为接口`AllUserFieldView`继承了接口`UserNameView`所以userName也会被输出。




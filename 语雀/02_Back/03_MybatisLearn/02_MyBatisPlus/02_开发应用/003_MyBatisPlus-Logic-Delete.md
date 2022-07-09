# MyBatisPlus-logic-Delete

## 1、加入表字段is_deleted 设置默认提供值0

![](003_MyBatisPlus-Logic-Delete.assets/image-20220312110357389.png)

## 2、实体类加注解

```java
@AllArgsConstructor
@NoArgsConstructor
@Data
public class User implements Serializable {

    @TableId("uid")
    private Long id;
    @TableField("user_name")
    private String name;
    private Integer age;
    private String email;
    @TableLogic
    private Integer isDeleted;
}

```


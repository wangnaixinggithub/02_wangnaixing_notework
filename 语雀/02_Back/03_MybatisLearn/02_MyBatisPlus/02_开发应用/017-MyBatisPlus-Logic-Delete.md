# MyBatisPlus-Logic-Delete

# 1、Need Config

```sql
 alter table tbl_employee add column is_deleted tinyint not null;
```

```java
@Data
@EqualsAndHashCode(callSuper = false)
@TableName("tbl_employee")
@ApiModel(value="TblEmployee对象", description="")
public class TblEmployee implements Serializable {

    private static final long serialVersionUID=1L;

    private Long id;

    private String lastName;

    private String email;

    private String gender;

    private Integer age;

    
    @TableLogic
    @TableField("is_deleted")
    private Boolean deleted;

}

```

# 2、Validation Test

```java
    @GetMapping("/delete")
    public CommonResult delete(){
        boolean b = employeeService.removeById(3);
        return CommonResult.success("");
    }
```

```
Preparing: UPDATE tbl_employee SET is_deleted=1 WHERE id=? AND is_deleted=0 
Parameters: 3(Integer)
```


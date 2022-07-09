# MyBatisPlus-LocalDateTimeFiled-Auto-Fill

# 1、Need Config

```sql
alter table tbl_employee add column gmt_create datetime not null;
alter table tbl_employee add column gmt_modified datetime not null;
```

```java
@Data
@EqualsAndHashCode(callSuper = false)
@TableName("tbl_employee")
public class TblEmployee implements Serializable {

    private static final long serialVersionUID=1L;

    private Long id;

    private String lastName;

    private String email;

    private String gender;

    private Integer age;

    @TableField(fill = FieldFill.INSERT) // 插入的时候自动填充
    private LocalDateTime gmtCreate;
    
    @TableField(fill = FieldFill.INSERT_UPDATE) // 插入和更新的时候自动填充
    private LocalDateTime gmtModified;
  

}
```

```java
@Component
@Slf4j
public class MyMetaObjectHandler implements MetaObjectHandler {

    @Override
    public void insertFill(MetaObject metaObject) {
        boolean hasGmtCreate = metaObject.hasSetter("gmtCreate");
        boolean hasGmtModified = metaObject.hasSetter("gmtModified");
        if (hasGmtCreate) {
            Object gmtCreate = this.getFieldValByName("gmtCreate", metaObject);
            if (gmtCreate == null) {
                this.strictInsertFill(metaObject, "gmtCreate", LocalDateTime.class, LocalDateTime.now());
            }
        }
        if (hasGmtModified) {
            Object gmtModified = this.getFieldValByName("gmtModified", metaObject);
            if (gmtModified == null) {
                this.strictInsertFill(metaObject, "gmtModified", LocalDateTime.class, LocalDateTime.now());
            }
        }
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        boolean hasGmtModified = metaObject.hasSetter("gmtModified");
        if (hasGmtModified) {
            Object gmtModified = this.getFieldValByName("gmtModified", metaObject);
            if (gmtModified == null) {
                this.strictInsertFill(metaObject, "gmtModified", LocalDateTime.class, LocalDateTime.now());
            }
        }
    }
}
```

# 2、Validation Test

```java
 	 @GetMapping("/insert")
    public CommonResult insert(){
        TblEmployee employee = new TblEmployee();
        employee.setLastName("lisa");
        employee.setEmail("lisa@qq.com");
        employee.setAge(20);
        employee.setDeleted(false);
        employeeService.save(employee);

        return CommonResult.success(employee);
    }
    
    @GetMapping("/update")
    public CommonResult update(){
        TblEmployee employee = new TblEmployee();
        employee.setId(1478548108547170306L);
        employee.setAge(50);
        employee.setDeleted(false);
        
        // 设置创建时间
        employee.setGmtModified(LocalDateTime.now());
        employeeService.updateById(employee);
        return CommonResult.success(employee);
    }
```

```javascript
2022-01-05 13:01:05.204 DEBUG 19020 --- [nio-8080-exec-1] c.w.m.m.p.m.TblEmployeeMapper.insert     : ==>  Preparing: INSERT INTO tbl_employee ( last_name, email, age, gmt_create, gmt_modified, is_deleted ) VALUES ( ?, ?, ?, ?, ?, ? ) 
2022-01-05 13:01:05.219 DEBUG 19020 --- [nio-8080-exec-1] c.w.m.m.p.m.TblEmployeeMapper.insert     : ==> Parameters: lisa(String), lisa@qq.com(String), 20(Integer), 2022-01-05T13:01:05.192(LocalDateTime), 2022-01-05T13:01:05.194(LocalDateTime), false(Boolean)
2022-01-05 13:01:05.236 DEBUG 19020 --- [nio-8080-exec-1] c.w.m.m.p.m.TblEmployeeMapper.insert     : <==    Updates: 1
2022-01-05 13:01:31.142 DEBUG 19020 --- [nio-8080-exec-2] c.w.m.m.p.m.T.updateById                 : ==>  Preparing: UPDATE tbl_employee SET age=?, gmt_modified=? WHERE id=? AND is_deleted=0 
2022-01-05 13:01:31.144 DEBUG 19020 --- [nio-8080-exec-2] c.w.m.m.p.m.T.updateById                 : ==> Parameters: 50(Integer), 2022-01-05T13:01:31.132(LocalDateTime), 1478548108547170306(Long)
2022-01-05 13:01:31.160 DEBUG 19020 --- [nio-8080-exec-2] c.w.m.m.p.m.T.updateById                 : <==    Updates: 1
```


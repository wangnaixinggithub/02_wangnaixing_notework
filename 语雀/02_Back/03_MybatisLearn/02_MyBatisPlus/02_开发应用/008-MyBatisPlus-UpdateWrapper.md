# MyBatisPlus-UpdateWrapper

```
1、设置更新字段

2、设置更新条件

3、and()，使用lambda语法，传入一个LambdaUpdateWrapper参数变量（本身）
```



```java
 @Test
    public void test(){
        //将（年龄大于20或邮箱为null）并且用户名中包含有a的用户信息修改
        LambdaUpdateWrapper<User> lambdaUpdateWrapper = Wrappers.<User>lambdaUpdate();
        //UPDATE t_user SET age=?,email=? WHERE is_deleted=0 AND (user_name LIKE ? AND (age > ? OR email IS NULL))
        lambdaUpdateWrapper.set(User::getAge,30).set(User::getEmail,"827376239@qq.com")
                .like(User::getName,"a").and(updateWrapper->updateWrapper.gt(User::getAge,20).or().isNull(User::getEmail));
        userMapper.update(null,lambdaUpdateWrapper);
    }
```


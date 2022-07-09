# MybatisPlus-Condition-Use-Conditon-Query

> 动态Sql用JAVA语言实现

```java
    @Test
    public void test08() {
//定义查询条件，有可能为null（用户未输入或未选择）
        String username = null;
        Integer ageBegin = 10;
        Integer ageEnd = 24;
        LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<User>();


        if(StringUtils.isNotBlank(username)){
            queryWrapper.like(User::getName,"a");
        }
        if(ageBegin != null){
            queryWrapper.ge(User::getAge, ageBegin);
        }
        if(ageEnd != null){
            queryWrapper.le(User::getAge, ageEnd);
        }
//SELECT id,username AS name,age,email,is_deleted FROM t_user WHERE (age >=? AND age <= ?)
        List<User> users = userMapper.selectList(queryWrapper);
        users.forEach(System.out::println);
    }
```

> 前面的代码可读性不高，使用MP提供的condition参数。

```java
    @Test
    public void test09(){
        String username = null;
        Integer ageBegin = 10;
        Integer ageEnd = 24;
        LambdaQueryWrapper<User> queryWrapper = new LambdaQueryWrapper<User>();

        queryWrapper.like(StringUtils.isNotBlank(username),User::getName,"a");
        queryWrapper.ge(ageBegin != null,User::getAge, ageBegin);
        queryWrapper.le(ageBegin != null,User::getAge, ageEnd);
//SELECT id,username AS name,age,email,is_deleted FROM t_user WHERE (age >=? AND age <= ?)
        List<User> users = userMapper.selectList(queryWrapper);
        users.forEach(System.out::println);

    }
```


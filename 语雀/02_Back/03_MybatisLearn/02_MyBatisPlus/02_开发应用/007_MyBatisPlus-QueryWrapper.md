# MyBatisPlus-QueryWrapper

## 1、多条件查询

```java
    /**
     * 多条件查询  SELECT uid AS id,user_name AS name,age,email,is_deleted FROM t_user WHERE is_deleted=0 AND (user_name LIKE ? AND age BETWEEN ? AND ? AND email IS NOT NULL)
     */
    @Test
    public void test01(){
        //查询用户名包含a，年龄在20到30之间，并且邮箱不为null的用户信息
        LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<User>();
        lambdaQueryWrapper.like(User::getName,'a').between(User::getAge,20,30).isNotNull(User::getEmail);
        userMapper.selectList(lambdaQueryWrapper);
    }
```

## 2、多字段排序查询

```java
    /**
     * 多字段排序查询
     * SELECT uid AS id,user_name AS name,age,email,is_deleted FROM t_user WHERE is_deleted=0 ORDER BY age DESC,uid ASC
     */
    @Test
    public void test02(){
        ///按年龄降序查询用户，如果年龄相同则按id升序排列
        LambdaQueryWrapper<User> lambdaQueryWrapper = Wrappers.<User>lambdaQuery();
        lambdaQueryWrapper.orderByDesc(User::getAge).orderByAsc(User::getId);
        userMapper.selectList(lambdaQueryWrapper);

    }
```

## 3、构造删除条件

```java
    /**
     * QueryWrapper 可以构造删除语句的where条件
     * UPDATE t_user SET is_deleted=1 WHERE is_deleted=0 AND (email IS NULL)
     */
    @Test
    public void test03(){
        //删除email为空的用户
        QueryWrapper<User> queryWrapper = new QueryWrapper<User>();
        queryWrapper.isNull("email");
        userMapper.delete(queryWrapper);
    }

```

## 4、构造更新条件以及Or的玩法

```java
    /**
     * QueryWrapper可以用在删除，更新这些DDL操作，作为他们的where条件，还可以用or
     * 
     * UPDATE t_user SET age=?, email=? WHERE is_deleted=0 AND (age > ? AND user_name LIKE ? OR email IS NULL)
     */
    @Test
    public void test04(){
        //将（年龄大于20并且用户名中包含有a）或邮箱为null的用户信息修改
        LambdaQueryWrapper<User> lambdaQueryWrapper = Wrappers.<User>lambdaQuery();
        lambdaQueryWrapper.gt(User::getAge,20).like(User::getName,"a").or().isNull(User::getEmail);
        User user = new User();
        user.setAge(18);
        user.setEmail("user@atguigu.com");

        userMapper.update(user,lambdaQueryWrapper);
    }
```

## 5、可以指定查询单表的哪些字段 

```java
    /**
     * Querywrapper 可以指定查询单表的哪些字段 
     * 
     * SELECT user_name AS name,age FROM t_user WHERE is_deleted=0
     */
    @Test
    public void test05(){
    //查询用户信息的username和age字段
        LambdaQueryWrapper<User> lambdaQueryWrapper = Wrappers.<User>lambdaQuery();
        lambdaQueryWrapper.select(User::getName,User::getAge);
        userMapper.selectList(lambdaQueryWrapper);
    }
```

## 6、还可以子查询

```java
   /**
     * 还提供了子查询
     *  SELECT uid AS id,user_name AS name,age,email,is_deleted FROM t_user WHERE is_deleted=0 AND (uid IN (select uid from t_user where uid <= 3))
     */
    @Test
    public void test06(){
        //查询id小于等于3的用户信息
        LambdaQueryWrapper<User> lambdaQueryWrapper = Wrappers.<User>lambdaQuery();
        lambdaQueryWrapper.inSql(User::getId,"select uid from t_user where uid <= 3");
        userMapper.selectList(lambdaQueryWrapper);
    }
```


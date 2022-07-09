# 025-MybatisPlus-IService-ServiceImpl

## 1、`IService<User>`

```java
/**
 * @author by wangnaixing
 * @Description UserService继承IService模板提供的基础功能
 * @Date 2022/3/10 23:34
 */
public interface UserService extends IService<User> {

    List<User> findAll();
}
```

## 2、ServiceImpl<UserMapper, User> 

```java
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService{


    @Override
    public List<User> findAll() {
     return baseMapper.selectList(null);
    }
}
```

## 2、Servcie layer API

```java
@SpringJUnitConfig(classes = {SpringConfig.class})
public class MybatisPlusTest {

    @Autowired
    private UserService userService;
    
    @Test
    public void testGetCount(){
        long count = userService.count();
        System.out.println("总记录数：" + count);
    }

    @Test
    public void testSaveBatch(){
// SQL长度有限制，海量数据插入单条SQL无法实行，
// 因此MP将批量插入放在了通用Service中实现，而不是通用Mapper
        ArrayList<User> users = new ArrayList<>();
        for (int i = 0; i < 5; i++) {
            User user = new User();
            user.setName("ybc" + i);
            user.setAge(20 + i);
            users.add(user);
        }
//SQL:INSERT INTO t_user ( username, age ) VALUES ( ?, ? )
        userService.saveBatch(users);
    }

}
```


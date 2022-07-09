# SpringBoot-Merge-JPA-Use-PagingAndSortingRepository

# 1、Need Config

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/jpa?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: root
    password: root
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    generate-ddl: true
    show-sql: true

```

```java
@Data
@Builder
@Accessors(chain = true)
@Entity
@AllArgsConstructor
@NoArgsConstructor
public class User {

    @Id
    @GeneratedValue(strategy= GenerationType.IDENTITY)
    private Long id;
    @Column
    private String name;
    @Column
    private String email;
}

```

```java
public interface UserRepository extends PagingAndSortingRepository<User,Long> {}
```

# 2、Validation Test

```java
@Slf4j
@SpringBootTest
class UserRepositoryTest {

    @Resource
    private UserRepository userRepository;


    private <T> void print(List<T> list) {
        if (!CollectionUtils.isEmpty(list)) {
            list.forEach(System.out::println);
        }
    }

    //测试JPA 单字段排序
    @Test
    void findAllBySort() {
        //select user0_.id as id1_0_, user0_.email as email2_0_, user0_.name as name3_0_ from user user0_
        // order by user0_.name asc
        Sort sort = Sort.by(Sort.Direction.ASC, "name");
        Iterable<User> users = userRepository.findAll(sort);
        users.forEach(System.out::println);

    }

    //测试JPA 多字段排序
    @Test
    void findAllBySort2() {
        //select user0_.id as id1_0_, user0_.email as email2_0_, user0_.name as name3_0_ from user user0_
        // order by user0_.name asc, user0_.id desc
        Sort sort = Sort.by(new Sort.Order(Sort.Direction.ASC, "name"),
                new Sort.Order(Sort.Direction.DESC, "id"));
        Iterable<User> users = userRepository.findAll(sort);
        users.forEach(System.out::println);
    }


    //测试JPA 查询带分页
    @Test
    void findAllByPageable() {
        //select user0_.id as id1_0_, user0_.email as email2_0_, user0_.name as name3_0_ from user user0_
        // limit ?, ?
        // select count(user0_.id) as col_0_0_ from user user0_


        //1.构造分页所属参数
        PageRequest request = PageRequest.of(1, 5);
        //2.得到分页对象
        Page<User> page = userRepository.findAll(request);
        //3.分页数据解析
        log.error("总条数 -------------> {}", page.getTotalElements());
        log.error("当前页数 -------------> {}", page.getNumber());
        log.error("当前每页显示数 -------------> {}", page.getSize());
        log.error("总页码-------------> {}", page.getTotalPages());
        print(page.getContent());


    }

    //========================继承了CrudRepository<T, ID> 接口，拥有了该接口全部方法==============================================//


    //测试JPA中的新增:底层会先查询！！！
    @Test
    void save() {
        //insert into user (email, name) values (?, ?)

        //1.构造User模型
        User user = User.builder()
                .name("wangnaixing")
                .email("827376239@qq.com")
                .build();
        //2.执行保存
        userRepository.save(user);
    }


    //测试JPA的批量保存：底层for循环，调用 save()
    @Test
    void saveAll() {
        //insert into user (email, name) values (?, ?) 底层跑了30条这样的SQL

        //1.构造User集合
        List<User> userList = new ArrayList<>();
        for (int i = 0; i < 30; i++) {
            User user = User.builder()
                    .name("wangnaixing" + i)
                    .email("827376239@qq.com")
                    .build();
            userList.add(user);
        }
        //2.执行批量保存
        userRepository.saveAll(userList);


    }

    //测试JPA根据主键ID查询，有就返回，没有返回null
    @Test
    void findById() {
        //select user0_.id as id1_0_0_, user0_.email as email2_0_0_, user0_.name as name3_0_0_ from user user0_
        // where user0_.id=?
        Long id = 1L;
        User user = userRepository.findById(id).orElse(null);
        System.out.println(user);
    }

    //测试JPA，此ID对应实体是否存在？ 在返回true 不在返回false
    @Test
    void existsById() {
        //select count(*) as col_0_0_ from user user0_
        // where user0_.id=?
        Long id = 1L;
        boolean flag = userRepository.existsById(id);
        System.out.println("flag=>" + flag);

    }

    //测试JPA 查询全部的方法
    @Test
    void findAll() {
        //select user0_.id as id1_0_, user0_.email as email2_0_, user0_.name as name3_0_ from user user0_
        List<User> users = (List<User>) userRepository.findAll();
        users.forEach(System.out::println);
    }

    //测试JPA 根据id构成的集合查询集合信息
    @Test
    void findAllById() {
        //select user0_.id as id1_0_, user0_.email as email2_0_, user0_.name as name3_0_ from user user0_ where user0_.id
        // in (? , ? , ? , ? , ?)

        List<Long> ids = new ArrayList<>();
        ids.add(1L);
        ids.add(2L);
        ids.add(3L);
        ids.add(4L);
        ids.add(5L);

        List<User> users = (List<User>) userRepository.findAllById(ids);
        users.forEach(System.out::println);
    }

    //测试JPA 查询表数量的方法
    @Test
    void count() {
        //select count(*) as col_0_0_ from user user0_
        long count = userRepository.count();
        System.out.println(count);
    }

    //测试JPA 根据ID删除
    @Test
    void deleteById() {

        //1. 底层先查询
        //select user0_.id as id1_0_0_, user0_.email as email2_0_0_, user0_.name as name3_0_0_ from user user0_
        // where user0_.id=?

        //2. 有，再执行删除操作
        //delete from user where id=?

        Long id = 3L;
        userRepository.deleteById(id);

    }

    //测试JPA根据实体集合批量删除
    @Test
    void deleteAllByEntity() {
        //底层：通过for循环调用 deleteById()

        /*Hibernate: select user0_.id as id1_0_0_, user0_.email as email2_0_0_, user0_.name as name3_0_0_ from user user0_ where user0_.id=?
Hibernate: select user0_.id as id1_0_0_, user0_.email as email2_0_0_, user0_.name as name3_0_0_ from user user0_ where user0_.id=?
Hibernate: select user0_.id as id1_0_0_, user0_.email as email2_0_0_, user0_.name as name3_0_0_ from user user0_ where user0_.id=?
Hibernate: select user0_.id as id1_0_0_, user0_.email as email2_0_0_, user0_.name as name3_0_0_ from user user0_ where user0_.id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?*/


        List<User> userList = new ArrayList<>();
        userList.add(new User(36L));
        userList.add(new User(37L));
        userList.add(new User(38L));
        userList.add(new User(39L));


        userRepository.deleteAll(userList);
    }

    //测试JPA 删除全部记录的方法
    @Test
    void deleteAll() {
        //底层：先调用findAll()得到全部实体，再for循环，调用deleteById() 一条一条删除！
        /*Hibernate: select user0_.id as id1_0_, user0_.email as email2_0_, user0_.name as name3_0_ from user user0_
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?
Hibernate: delete from user where id=?*/

        userRepository.deleteAll();
    }


}

```


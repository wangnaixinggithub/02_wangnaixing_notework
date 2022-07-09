# SpringBoot-Merge-JPA-Query-Anno-Use

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
public class User implements Serializable {

    @Id
    @GeneratedValue(strategy= GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;
    private String sex;
    private String address;
    private Date birth;
    private Integer age;

}
```

```java
public interface UserRepository extends JpaRepository<User,Long> {



    /**
     * 根据姓名和地址查询用户集合
     * @param name 姓名
     * @param address 地址
     * @return
     */
    @Query("select u from User u where u.name = ?1 and u.address = ?2")
    List<User> findByNameAndAddress(String name,String address);

    /**
     * 根据姓名或者地址查询用户集合
     * @param name 姓名
     * @param address 地址
     * @return
     */
    @Query("select u from User u where u.name = ?1 or u.address = ?2")
    List<User> findByNameOrAddress(String name,String address);


    /**
     * 根据姓名查询用户集合
     * @param name 姓名
     * @return
     */
    @Query("select u from User u where u.name = ?1")
    List<User> findByName(String name);


    /**
     * 根据姓名查询用户集合
     * @param name 姓名
     * @return
     */
    @Query("select u from User u where u.name = ?1")
    List<User> findByNameIs(String name);


    /**
     * 根据姓名查询用户集合
     * @param name 姓名
     * @return
     */
    @Query("select u from User u where u.name = ?1")
    List<User> findByNameEquals(String name);


    /**
     * 查询用户的生日日期在某个范围
     * @param date1 开始日期
     * @param date2 终止日期
     * @return
     */
    @Query("select u from User u where u.birth between ?1 and ?2")
    List<User> findByBirthBetween(Date date1, Date date2);

    /**
     * 查询用户生日小于某个范围
     * @param date 最大值
     * @return
     */
    @Query("select u from User u where u.birth < ?1")
    List<User> findByBirthLessThan(Date date);

    /**
     * 查询用户生日小于某个范围
     * @param date 最大值
     * @return
     */
    @Query("select u from User u where u.birth < ?1")
    List<User> findByBirthBefore(Date date);

    /**
     * 查询用户生日小于等于某个范围
     * @param date 最大值
     * @return
     */
    @Query("select u from User u where u.birth <= ?1")
    List<User> findByBirthLessThanEqual(Date date);

    /**
     * 查询用户生日大于某个范围
     * @param date 最小值
     * @return
     */
    @Query("select u from User u where u.birth > ?1")
    List<User> findByBirthGreaterThan(Date date);

    /**
     * 查询用户生日大于某个范围
     * @param date 最小值
     * @return
     */
    @Query("select u from User u where u.birth > ?1")
    List<User> findByBirthAfter(Date date);

    /**
     * 查询用户生日大于等于某个范围
     * @param date 最小值
     * @return
     */
    @Query("select u from User u where u.birth >= ?1")
    List<User> findByBirthGreaterThanEqual(Date date);

    /**
     * 查找姓名为空的用户集合
     * @param
     * @return
     */
    @Query("select u from User u where u.name is null ")
    List<User> findByNameIsNull();

    /**
     * 查找姓名不为空的用户集合
     * @return
     */
    @Query("select u from User u where u.name is not null ")
    List<User> findByNameIsNotNull();


    /**
     * 查找姓名不为空的用户集合
     * @return
     */
    @Query("select u from User u where u.name is not null ")
    List<User> findByNameNotNull();

    /**
     * 根据姓名查询用户集合
     * @param name 姓名
     * @return
     */
    @Query("select u from User u where u.name like ?1 ")
    List<User> findByNameLike(String name);

    /**
     *  根据姓名前模糊查询 %name
     * @param name
     * @return
     */
    @Query("select u from User u where u.name like %?1 ")
    List<User> findByNameStartingWith(String name);
    /**
     *  根据姓名后模糊查询 name%
     * @param name
     * @return
     */
    @Query("select u from User u where u.name like ?1% ")
    List<User> findByNameEndingWith(String name);
    /**
     *  根据姓名全模糊查询 %name%
     * @param name
     * @return
     */
    @Query("select u from User u where u.name like %?1% ")
    List<User> findByNameContaining(String name);

    /**
     * 根据姓名查询，并按照名字排序
     * @return
     */
    @Query("select u from User u where u.name = ?1 order by name desc ")
    List<User> findByNameOrderByNameDesc(String name);

    /**
     * 查询姓名不等于 name 的用户集合
     * @param name 姓名
     * @return
     */
    @Query("select u from User u where u.name <> ?1  ")
    List<User> findByNameNot(String name);

    /**
     * 查询用户年龄在集合指定范围之内的用户信息
     * @param ages
     * @return
     */
    List<User> findByAgeIn(Collection<Integer> ages);
    /**
     * 查询用户年龄在集合不在指定范围之内的用户信息
     * @param ages
     * @return
     */
    List<User> findByAgeNotIn(Collection<Integer> ages);

    /**
     * 根据用户名查询，并排序
     * @param name
     * @return
     */
    @Query("select u from User u where u.name = ?1 ")
    List<User> findByName(String name,Sort sort);

    /**
     * 根据用户名查询，并分页
     * @param name 姓名
     * @param pageable 分页参数
     * @return
     */
    @Query("select u from User u where u.name =?1")
    Page<User> findByName(String name, Pageable pageable);


}

```

# 2、Validation Test

```java
@SpringBootTest
class UserRepositoryTest {

    @Resource
    private UserRepository userRepository;


    private <T> void print(List<T> list) {
        if (!CollectionUtils.isEmpty(list)) {
            list.forEach(System.out::println);
        }
    }

    // and 查询条件
    @Test
    void  findByNameAndAddress(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.email as email3_0_, user0_.name as name4_0_, user0_.sex as sex5_0_ from user user0_
        // where user0_.name=? and user0_.address=?
        String name = "jack1";
        String address= "shanghai";
        List<User> userList = userRepository.findByNameAndAddress(name, address);

    }
    // or 查询条件
    @Test
    void findByNameOrAddress(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.email as email3_0_, user0_.name as name4_0_, user0_.sex as sex5_0_ from user user0_
        // where user0_.name=? or user0_.address=?
        String name = "jack1";
        String address= "shanghai";
        List<User> userList = userRepository.findByNameOrAddress(name, address);
    }
    // = 查询条件
    @Test
    void findByName(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.email as email3_0_, user0_.name as name4_0_, user0_.sex as sex5_0_ from user user0_
        // where user0_.name=?
        String name = "jack1";
        List<User> userList = userRepository.findByName(name);
    }
    // = 查询条件
    @Test
    void findByNameIs(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.email as email3_0_, user0_.name as name4_0_, user0_.sex as sex5_0_ from user user0_
        // where user0_.name=?
        String name = "jack1";
        List<User> userList = userRepository.findByNameIs(name);
    }
    // = 查询条件
    @Test
    void findByNameEquals(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.email as email3_0_, user0_.name as name4_0_, user0_.sex as sex5_0_ from user user0_
        // where user0_.name=?
        String name = "jack1";
        List<User> userList = userRepository.findByNameEquals
                (name);
    }

    //  between ? and ? 查询条件
    @Test
    void findByBirthBetween(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.birth between ? and ?
        DateTime startDate = DateUtil.parse("2019-12-07 19:22:33");
        DateTime endDate = DateUtil.parse("2022-12-01 19:11:33");
        List<User> userList = userRepository.findByBirthBetween(startDate, endDate);
    }
    //   < 查询条件
    @Test
    void findByBirthLessThan(){
        //elect user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.birth<?
        DateTime startDate = DateUtil.parse("2019-12-07 19:22:33");
        List<User> userList = userRepository.findByBirthLessThan(startDate);
    }
    //   < 查询条件
    @Test
    void findByBirthBefore(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.birth<?
        DateTime startDate = DateUtil.parse("2019-12-07 19:22:33");
        List<User> userList = userRepository.findByBirthBefore(startDate);
    }

    // <= 查询条件
    @Test
    void findByBirthLessThanEqual(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.birth<=?
        DateTime startDate = DateUtil.parse("2019-12-07 19:22:33");
        List<User> userList = userRepository.findByBirthLessThanEqual(startDate);
    }
    // > 查询条件
    @Test
    void findByBirthGreaterThan(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.birth>?
        DateTime startDate = DateUtil.parse("2019-12-07 19:22:33");
        List<User> userList = userRepository.findByBirthGreaterThan(startDate);

    }
    // >= 查询条件
    @Test
    void findByBirthGreaterThanEqual(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.birth>=?
        DateTime startDate = DateUtil.parse("2019-12-07 19:22:33");
        List<User> userList = userRepository.findByBirthGreaterThanEqual(startDate);
    }
    // > 大于查询条件
    @Test
    void findByBirthAfter(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.birth>?
        DateTime startDate = DateUtil.parse("2019-12-07 19:22:33");
        List<User> userList = userRepository.findByBirthAfter(startDate);
    }
    // is null 字段为空的查询条件
    @Test
    void findByNameIsNull(){
// select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
// where user0_.name is null
        List<User> userList = userRepository.findByNameIsNull();
    }
    // is not null 字段不为空的查询条件
    @Test
    void findByNameIsNotNull(){
//     select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
//        where user0_.name is not null
        List<User> userList = userRepository.findByNameIsNotNull();
    }

    // is not null 等价
    @Test
    void findByNameNotNull(){
        //elect user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.name is not null
        List<User> userList = userRepository.findByNameNotNull();
    }
    // ? = jac
    @Test
    void findByNameLike(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.name like ? escape ?
        // ? = jac
        String name = "jac";
        List<User> userList = userRepository.findByNameLike(name);
        print(userList);
    }
    // ? = %jac
    @Test
    void findByNameStartingWith(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.name like ? escape ?
        // ? = %jac
        String name = "jac";
        List<User> userList = userRepository.findByNameStartingWith(name);
        print(userList);
    }
    // ? = jac%
    @Test
    void findByNameEndingWith(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.name like ? escape ?
        // ? = jac%
        String name = "jac";
        List<User> userList = userRepository.findByNameEndingWith(name);
        print(userList);
    }
    // ? = %jac%
    @Test
    void findByNameContaining(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.name like ? escape ?
        // ? = %jac%
        String name = "jac";
        List<User> userList = userRepository.findByNameContaining(name);
        print(userList);
    }
    // orderBy
    @Test
    void findAllOrderByNameDesc(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // order by user0_.name desc
        List<User> userList = userRepository.findAll(Sort.by(new Sort.Order(Sort.Direction.DESC, "name")));
    }
    //orderBy
    @Test
    void findByNameOrderByNameDesc(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.name=?
        // order by user0_.name desc
        String name = "jac";
        List<User> userList = userRepository.findByNameOrderByNameDesc(name);
        print(userList);
    }
    @Test
    void findByNameNot(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.birth as birth3_0_, user0_.email as email4_0_, user0_.name as name5_0_, user0_.sex as sex6_0_ from user user0_
        // where user0_.name<>?
        String name = "jack01";
        List<User> userList = userRepository.findByNameNot(name);
        print(userList);

    }

    @Test
    void findByAgeIn(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.age as age3_0_, user0_.birth as birth4_0_, user0_.email as email5_0_, user0_.name as name6_0_, user0_.sex as sex7_0_ from user user0_
        // where user0_.age in (? , ? , ?)
        List<Integer> list = new ArrayList<>();
        list.add(12);
        list.add(13);
        list.add(15);

        List<User> userList = userRepository.findByAgeIn(list);
        print(userList);
    }
    @Test
    void findByAgeNotIn(){
        // select user0_.id as id1_0_, user0_.address as address2_0_, user0_.age as age3_0_, user0_.birth as birth4_0_, user0_.email as email5_0_, user0_.name as name6_0_, user0_.sex as sex7_0_ from user user0_
        // where user0_.age not in  (? , ? , ?)
        List<Integer> list = new ArrayList<>();
        list.add(12);
        list.add(13);
        list.add(15);
        List<User> userList = userRepository.findByAgeNotIn(list);
        print(userList);
    }

    @Test
    void finByNameSort(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.age as age3_0_, user0_.birth as birth4_0_, user0_.email as email5_0_, user0_.name as name6_0_, user0_.sex as sex7_0_ from user user0_
        // where user0_.name=? order by ? desc
            List<User> byName = userRepository.findByName("jack01", Sort.by(new Sort.Order(Sort.Direction.DESC,"age")));
    }
    @Test
    void findByNamePage(){
    //Hibernate: select user0_.id as id1_0_, user0_.address as address2_0_, user0_.age as age3_0_, user0_.birth as birth4_0_, user0_.email as email5_0_, user0_.name as name6_0_, user0_.sex as sex7_0_ from user user0_ where user0_.name=? limit ?, ?
    //Hibernate: select count(user0_.id) as col_0_0_ from user user0_ where user0_.name=?
        String name = "jack1";
        PageRequest request = PageRequest.of(1, 2);
        Page<User> page = userRepository.findByName(name, request);
    }



}
```


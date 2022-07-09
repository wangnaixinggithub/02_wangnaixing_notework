# SpringBoot-Merge-JPA-Sort-And-Pageable

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
     * 根据姓名分页查询
     * @param name 姓名
     * @param pageable 分页参数对象
     * @return 分页对象，包括分页数据，总页码，总记录数。当前页，每页大小这些数据
     */
    // Page<User> findByNameContaining(String name, Pageable pageable);

    /**
     *根据姓名分页查询
     * @param name 姓名
     * @param pageable 分页参数对象
     * @return 得到当前分页片，包括分页数据，前页，每页大小这些数据
     */
    //Slice<User> findByNameContaining(String name, Pageable pageable);

    /**
     *  根据姓名分页查询
     * @param name 姓名
     * @param pageable 分页参数对象
     * @return 得到当前分页数据
     */
    //List<User> findByNameContaining(String name, Pageable pageable);




    /**
     * 根据姓名查询用户信息并排序
     * @param name
     * @param sort
     * @return
     */
    List<User> findByName(String name, Sort sort);

    /**
     * 根据姓名查询 只要匹配到的第一条
     * @param name 姓名
     * @return
     */
    User findFirstByName(String name);

    /**
     *  根据姓名查询 只要匹配到的前两条
     * @param name 姓名
     * @return
     */
    List<User> findTop2ByName(String name);


}

```

# 2、Validation Test

```java
package com.wnx.mall.tiny.repository;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.wnx.mall.tiny.entity.User;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;

import org.springframework.data.domain.PageRequest;

import org.springframework.data.domain.Sort;
import org.springframework.util.CollectionUtils;

import javax.annotation.Resource;
import java.util.List;
import java.util.stream.Stream;

@SpringBootTest
class UserRepositoryTest {

    @Resource
    private UserRepository userRepository;



    private <T> void print(List<T> list) {
        if (!CollectionUtils.isEmpty(list)) {
            list.forEach(System.out::println);
        }
    }


    //JPA 根据姓名分页查询
    @Test
    void findByNameUsrPage() throws JsonProcessingException {
        String name = "jack";
        PageRequest request = PageRequest.of(1, 3);

        //1.Page 作为接受值，底层跑两条sql
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.age as age3_0_, user0_.birth as birth4_0_, user0_.email as email5_0_, user0_.name as name6_0_, user0_.sex as sex7_0_ from user user0_ where user0_.name like ? escape ?
        // limit ?, ?
        //select count(user0_.id) as col_0_0_ from user user0_ where user0_.name like ? escape ?
        //Page<User> page = userRepository.findByNameContaining(name, request);

      
        //2.Slice 作为接受值，底层只跑查询sql

        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.age as age3_0_, user0_.birth as birth4_0_, user0_.email as email5_0_, user0_.name as name6_0_, user0_.sex as sex7_0_ from user user0_ where user0_.name like ? escape ?
        // limit ?, ?
        //Slice<User> page = userRepository.findByNameContaining(name, request);
        
        //3.List<T> 作为接受值，底层智跑查询sql,分页数据封装到List中！
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.age as age3_0_, user0_.birth as birth4_0_, user0_.email as email5_0_, user0_.name as name6_0_, user0_.sex as sex7_0_ from user user0_ where user0_.name like ? escape ?
        // limit ?, ?
        //List<User> userList = userRepository.findByNameContaining(name, request);
        //print(userList);
        
    }
    @Test
    void findByNameUseSort(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.age as age3_0_, user0_.birth as birth4_0_, user0_.email as email5_0_, user0_.name as name6_0_, user0_.sex as sex7_0_ from user user0_ where user0_.name=?
        // order by user0_.age desc
        String name = "jack";
        PageRequest request = PageRequest.of(1, 3);
        Sort sort = Sort.by(new Sort.Order(Sort.Direction.DESC, "age"));
       // List<User> userList = userRepository.findByName(name,sort);
    }

    @Test
    void findFirstByName(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.age as age3_0_, user0_.birth as birth4_0_, user0_.email as email5_0_, user0_.name as name6_0_, user0_.sex as sex7_0_ from user user0_
        // where user0_.name=?
        // limit 1
        String name = "jack1";
        User user = userRepository.findFirstByName(name);
        System.out.println(user);
    }

    @Test
    void findTop2ByName(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.age as age3_0_, user0_.birth as birth4_0_, user0_.email as email5_0_, user0_.name as name6_0_, user0_.sex as sex7_0_ from user user0_
        // where user0_.name=?
        // limit 2
        String name = "jack1";
        List<User> users = userRepository.findTop2ByName(name);
        System.out.println(users);
    }
}

```


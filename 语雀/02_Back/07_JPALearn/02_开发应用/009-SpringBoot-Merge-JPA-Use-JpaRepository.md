# SpringBoot-Merge-JPA-Use-JpaRepository

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

}
```

```java
@Data
@Builder
@AllArgsConstructor
@Accessors(chain = true)
public class UserDto {
    private String name,email;
}

```

```java
public interface UserRepository extends JpaRepository<User,Long> {

    /**
     * 单条件查询
     * @param name 姓名
     * @return
     */
    List<User> findByName(String name);

    /**
     * 多条件查询
     * @param email 邮件
     * @param name 姓名
     * @return
     */
    List<User> findByEmailAndName(String email, String name);

    /**
     * 查询后用Dto接收 返回值
     * @param email 邮件
     * @return
     */
    List<UserDto> findByEmail(String email);


    /**
     * and的查询关系
     * @param email 邮件
     * @param address 地址
     * @return
     */
    List<User> findByEmailAndAddress(String email,String address);

    /**
     * distinct 去重
     * @param name 姓名
     * @return
     */
    List<User> findDistinctByName(String name);

    /**
     * or的查询关系
     * @param name 姓名
     * @param address 地址
     * @return
     */
    List<User> findByNameOrAddress(String name,String address);

    /**
     * 忽略大小写的查询关系
     * @param name 姓名
     * @return
     */
    List<User>findByNameIgnoreCase(String name);

    /**
     * 排序关系
     * @param name 姓名
     * @return
     */
    List<User> findByNameOrderByNameDesc(String name);



}
```

# 2、Validation Test

```java
package com.wnx.mall.tiny.repository;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.wnx.mall.tiny.entity.User;
import com.wnx.mall.tiny.entity.UserDto;
import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.util.CollectionUtils;

import javax.annotation.Resource;
import java.util.ArrayList;
import java.util.List;

@Slf4j
@SpringBootTest
class UserRepositoryTest {
    @Resource
    private UserRepository userRepository;
    @Resource
    private ObjectMapper objectMapper;



    private <T> void print(List<T> list) {
        if (!CollectionUtils.isEmpty(list)) {
            list.forEach(System.out::println);
        }
    }

    //测试JPA的批量删除方法
    @Test
    void deleteInBatch(){
         //批量删除：方法用了or关键字
        //delete from user where id=? or id=? or id=? or id=?

        List<User> userList = new ArrayList<>();
        userList.add(new User(60L));
        userList.add(new User(61L));
        userList.add(new User(62L));
        userList.add(new User(63L));

        userRepository.deleteInBatch(userList);
    }

    //测试JPA 单条件查询
    @Test
    void selectWhere(){
        //select user0_.id as id1_0_, user0_.email as email2_0_, user0_.name as name3_0_ from user user0_
        // where user0_.name=?
        String name = "wangnaixing0";
        List<User> userList = userRepository.findByName(name);
        print(userList);

    }

    //测试JPA 多条件查询
    @Test
    void selectWhere2(){
        // select user0_.id as id1_0_, user0_.email as email2_0_, user0_.name as name3_0_ from user user0_
        // where user0_.email=? and user0_.name=?
        String name = "wangnaixing0";
        String email = "827376239@qq.com";
        List<User> userList = userRepository.findByEmailAndName(email, name);
        print(userList);
    }

    //测试JPA用Dto接受返回值
    @Test
    void selectReturnDto(){
        //select user0_.name as col_0_0_, user0_.email as col_1_0_ from user user0_ where user0_.email=?
        String email = "827376239@qq.com";
        List<UserDto> userDto = userRepository.findByEmail(email);
        print(userDto);

    }




    //测试全表记录删除
    @Test
    void deleteAllInBatch(){
        //delete from user
        userRepository.deleteAllInBatch();
    }

    //测试JPA and的查询关系
    @Test
    void findByEmailAndAddress(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.email as email3_0_, user0_.name as name4_0_, user0_.sex as sex5_0_ from user user0_
        // where user0_.email=? and user0_.address=?
        String email = "123456@126.com";
        String address=  "shanghai";
        List<User> userList = userRepository.findByEmailAndAddress(email, address);
        print(userList);
    }
    //测试JPA 去重distinct
    @Test
    void findDistinctByName(){
        // select distinct
        // user0_.id as id1_0_, user0_.address as address2_0_, user0_.email as email3_0_, user0_.name as name4_0_, user0_.sex as sex5_0_ from user user0_
        // where user0_.name=?
        String name = "jack1";
        List<User> userList = userRepository.findDistinctByName(name);
        print(userList);
    }

    //测试JPA  or的查询关系
    @Test
    void findByNameOrAddress(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.email as email3_0_, user0_.name as name4_0_, user0_.sex as sex5_0_ from user user0_
        // where user0_.name=? or user0_.address=?
        String name = "jack1";
        String address = "123456@126.com";
        List<User> userList = userRepository.findByNameOrAddress(name, address);
        print(userList);
    }
    //测试JPA 忽略大小写的查询关系
    @Test
    void findByNameIgnoreCase(){
        // select user0_.id as id1_0_, user0_.address as address2_0_, user0_.email as email3_0_, user0_.name as name4_0_, user0_.sex as sex5_0_ from user user0_
        // where upper(user0_.name)=upper(?)
        String name = "jack1";
        List<User> userList = userRepository.findByNameIgnoreCase(name);
        print(userList);
    }
    //测试JPA orderBy 的查询关系
    @Test
    void findAllOrderByName(){
        //select user0_.id as id1_0_, user0_.address as address2_0_, user0_.email as email3_0_, user0_.name as name4_0_, user0_.sex as sex5_0_ from user user0_
        // where user0_.name=? order by user0_.name desc
        String name = "jack1";
        List<User> userList = userRepository.findByNameOrderByNameDesc(name);
        print(userList);

    }





}

```


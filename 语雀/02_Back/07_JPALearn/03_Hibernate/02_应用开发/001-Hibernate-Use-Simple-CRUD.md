# Hibernate-Use-Simple-CRUD

```java
public interface EntityManager {
     //1.新增
	 public void persist(Object entity);
    //2.有ID就是修改，无ID就是新增
	 public <T> T merge(T entity);
    //3.删除
	 public void remove(Object entity);
    //4.查询
	 public <T> T find(Class<T> entityClass, Object primaryKey);
}
```

```java
package com.wnx.spring;

import com.wnx.spring.model.User;
import lombok.extern.log4j.Log4j2;
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

@Log4j2
public class JpaTest {
    EntityTransaction transaction;
    EntityManager entityManager;
    EntityManagerFactory entityManagerFactory;
 
    @BeforeEach
    public void beforeEach(){
         entityManagerFactory = Persistence.createEntityManagerFactory("NewPersistenceUnit");
         entityManager = entityManagerFactory.createEntityManager();
         transaction = entityManager.getTransaction();
        transaction.begin();
    }

    @AfterEach
    public void afterEach(){
        transaction.commit();
        entityManager.close();
        entityManagerFactory.close();
    }
    
    @Test
    public void persistTest() {
        User user = User.builder()
                .userId(null)
                .userName("张大麻子")
                .userPwd("123")
                .userEmail("827376239@qq.com")
                .age(30).build();
        
        entityManager.persist(user);    //新增

        //Hibernate:
        //    select
        //        next_val as id_val
        //    from
        //        hibernate_sequence for update
        //

        //Hibernate:
        //    update
        //        hibernate_sequence
        //    set
        //        next_val= ?
        //    where
        //        next_val=?

        //Hibernate:
        //    insert
        //    into
        //        t_user
        //        (age, user_email, user_name, user_pwd, userId)
        //    values
        //        (?, ?, ?, ?, ?)

    }

    @Test
    public void mergeTest() {
        User user = User.builder()
                .userId(null)
                .userName("张大麻子")
                .userPwd("123")
                .userEmail("827376239@qq.com")
                .age(30).build();       //create Operation Can rollBack primaryKey

        //Hibernate:            
        //    select
        //        user0_.userId as userid1_0_0_,
        //        user0_.age as age2_0_0_,
        //        user0_.user_email as user_ema3_0_0_,
        //        user0_.user_name as user_nam4_0_0_,
        //        user0_.user_pwd as user_pwd5_0_0_
        //    from
        //        t_user user0_
        //    where
        //        user0_.userId=?

        //Hibernate:
        //    select
        //        next_val as id_val
        //    from
        //        hibernate_sequence for update
        //

        //Hibernate:
        //    update
        //        hibernate_sequence
        //    set
        //        next_val= ?
        //    where
        //        next_val=?

        //Hibernate:
        //    insert
        //    into
        //        t_user
        //        (age, user_email, user_name, user_pwd, userId)
        //    values
        //        (?, ?, ?, ?, ?)

        log.info("before => {}",user);
        //[2022-05-15 11:32:08:572] [INFO] - com.wnx.spring.JpaTest.mergeTest(JpaTest.java:114) - before => User(userId=null, userName=张大麻子, userPwd=123, userEmail=827376239@qq.com, age=30)
        User merge = entityManager.merge(user);

        log.info("after => {}",merge);
        //   [2022-05-15 11:32:08:612] [INFO] - com.wnx.spring.JpaTest.mergeTest(JpaTest.java:118) - after => User(userId=17, userName=张大麻子, userPwd=123, userEmail=827376239@qq.com, age=30)

    }

    @Test
    public void mergeTest2() {
        User user = User.builder()
                .userId(17L)
                .userName("王乃醒大帅哥")
                .userPwd("123456789")
                .userEmail("827376239@qq.com")
                .age(26).build();         //update Operation If primaryKey exists! final ResultModel is UpdateNew

        User merge = entityManager.merge(user);

        log.info("{}",merge);

        //[2022-05-15 11:49:44:055] [INFO] - com.wnx.spring.JpaTest.mergeTest2(JpaTest.java:134) - User(userId=17, userName=王乃醒大帅哥, userPwd=123456789, userEmail=827376239@qq.com, age=26)

        //Hibernate:
        //    select
        //        user0_.userId as userid1_0_0_,
        //        user0_.age as age2_0_0_,
        //        user0_.user_email as user_ema3_0_0_,
        //        user0_.user_name as user_nam4_0_0_,
        //        user0_.user_pwd as user_pwd5_0_0_
        //    from
        //        t_user user0_
        //    where
        //        user0_.userId=?

        //Hibernate:
        //    update
        //        t_user
        //    set
        //        age=?,
        //        user_email=?,
        //        user_name=?,       字段值相同的不会去改！
        //        user_pwd=?
        //    where
        //        userId=?

    }


    @Test
    public void findTest() {
        User user = entityManager.find(User.class, 17L);

        //Hibernate:
        //    select
        //        user0_.userId as userid1_0_0_,
        //        user0_.age as age2_0_0_,
        //        user0_.user_email as user_ema3_0_0_,
        //        user0_.user_name as user_nam4_0_0_,
        //        user0_.user_pwd as user_pwd5_0_0_
        //    from
        //        t_user user0_
        //    where
        //        user0_.userId=?


        log.info("=> {}  ",user);

    //[2022-05-15 11:53:26:304] [INFO] - com.wnx.spring.JpaTest.findTest(JpaTest.java:168) - => User(userId=17, userName=王乃醒大帅哥, userPwd=123456789, userEmail=827376239@qq.com, age=26)      
    }

    @Test
    public void removeTest() {
        //必须先查出来，才能删！
        User removeUser = entityManager.find(User.class, 18L);

        entityManager.remove(removeUser);

        //Hibernate:
        //    delete
        //    from
        //        t_user
        //    where
        //        userId=?
    }

    @Test
    public void updateTest() {      //更新操作
        User removeUser = entityManager.find(User.class, 17L);
        removeUser.setUserName("xxx");
        //Hibernate:
        //    select
        //        user0_.userId as userid1_0_0_,
        //        user0_.age as age2_0_0_,
        //        user0_.user_email as user_ema3_0_0_,
        //        user0_.user_name as user_nam4_0_0_,
        //        user0_.user_pwd as user_pwd5_0_0_
        //    from
        //        t_user user0_
        //    where
        //        user0_.userId=?


        //Hibernate:
        //    update
        //        t_user
        //    set
        //        age=?,
        //        user_email=?,
        //        user_name=?,
        //        user_pwd=?
        //    where
        //        userId=?

    }
    

}
```

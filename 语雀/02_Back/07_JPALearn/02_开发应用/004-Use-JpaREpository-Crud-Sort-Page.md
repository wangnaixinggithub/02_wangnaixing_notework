# Use-JpaRepository-Crud-Sort-Page

```java
public interface UserDao  extends JpaRepository<User,Long> {}

```

# @Autowried is SimpleJpaRepository

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    @Autowired
    private UserDao userDao;
    @Test
    public void test(){
        //org.springframework.data.jpa.repository.support.SimpleJpaRepository@5b84f14
        System.out.println(userDao);
    }
}
```

# 1、save()

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    @Autowired
    private UserDao userDao;
    @Test
    public void test(){
        userDao.save(User.builder()
                     .userId(null)
                     .userName("王乃醒")
                     .userStatus("正常")
                     .build()
                    );
    }
}
```

```java
public class SimpleJpaRepository<T, ID extends Serializable> implements JpaRepository<T, ID>, JpaSpecificationExecutor<T> {
	
    @Transactional
	public <S extends T> S save(S entity) {
		// save And Update 二合一
        if (entityInformation.isNew(entity)) {
			
            em.persist(entity);
            return entity;
            
		} else {
			
            return em.merge(entity);
		}
        
	}
}
```

# 2、saveAndFlush()

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    
    @Autowired
    private UserDao userDao;
    
    @Test
    public void test(){
        userDao.saveAndFlush(User.builder()
                             .userId(null)
                             .userName("王乃醒")
                             .userStatus("正常")
                             .build()
                            );
    }

}

```



```java
@Repository
@Transactional(readOnly = true)
public class SimpleJpaRepository<T, ID extends Serializable> implements JpaRepository<T, ID>, JpaSpecificationExecutor<T> {	
	@Transactional
	public <S extends T> S saveAndFlush(S entity) {
        
        //底层跑save() 逻辑
		S result = save(entity);
        
		//多了一步，刷新实体管理器
        flush();
		
        return result;
	}
	
	@Transactional
	public void flush() {

		em.flush();
	}
}
```

# 3、save(Iterable`<S>`  entities)

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    @Autowired
    private UserDao userDao;
    
    @Test
    public void test(){
        List<User> userList = new ArrayList<>();
        userList.add( User.builder().userId(null).userName("王乃醒1").userStatus("正常").build());
        userList.add( User.builder().userId(null).userName("王乃醒2").userStatus("正常").build());
        userList.add( User.builder().userId(null).userName("王乃醒3").userStatus("正常").build());
        
        userDao.save(userList);
   
    }

}
```

```java
@Repository
@Transactional(readOnly = true)
public class SimpleJpaRepository<T, ID extends Serializable> implements JpaRepository<T, ID>, JpaSpecificationExecutor<T> {	
   
    @Transactional
	public <S extends T> List<S> save(Iterable<S> entities) {
		List<S> result = new ArrayList<S>();
		
        if (entities == null) {return result;}
		
        //底层是通过For循环
        for (S entity : entities) {
			result.add(save(entity));
		}
        
        //然后把新增用户信息放到result中返回了
		return result;
	}
}
```

# 4、 delete(ID id)

- 根据实体ID删除实体，如果是根据ID删除，底层会先查询出来实体。再删除。

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    @Autowired
    private UserDao userDao;
   
    @Test
    public void test(){
        
        userDao.delete(18L);
        
    }
}
```

# 5、delete(T entity)

```java
public class SimpleJpaRepository<T, ID extends Serializable>

	public void delete(ID id) {
		Assert.notNull(id, ID_MUST_NOT_BE_NULL);
    	
        //1.先查询实体
		T entity = findOne(id);
    
		if (entity == null) {
			throw new EmptyResultDataAccessException(
					String.format("No %s entity with id %s exists!", entityInformation.getJavaType(), id), 1);
		}
    
    	//2.再删除实体
		delete(entity);
	}

	@Transactional
	public void delete(T entity) {
		Assert.notNull(entity, "The entity must not be null!");
        
		em.remove(em.contains(entity) ? entity : em.merge(entity));
	}
}
```

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    
    @Autowired
    private UserDao userDao;
    
    @Test
    public void test(){
        userDao.delete(User.builder().userId(16L).userName("王乃醒1").userStatus("正常").build());
    }
    
}
```

# 6、delete(`Iterable<? extends T>` entities)

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    @Autowired
    private UserDao userDao;
    @Test
    public void test(){
        List<User> userList = new ArrayList<>();
        userList.add(User.builder().userId(14L).userName("王乃醒").userStatus("正常").build());
        userList.add(User.builder().userId(15L).userName("王乃醒").userStatus("正常").build());
        
        userDao.delete(userList);
    }
}
```

```java
	@Transactional
	public void delete(Iterable<? extends T> entities) {
		Assert.notNull(entities, "The given Iterable of entities not be null!");
	
        //调用删除方法，相当于先查询，查询到了再删除。
        for (T entity : entities) {
			delete(entity);
		}
	}
```

# 7、deleteInBatch(`Iterable<T>` entities)

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    @Autowired
    private UserDao userDao;
  
    @Test
    public void test(){
        List<User> userList = new ArrayList<>();
        userList.add(User.builder().userId(13L).userName("王乃醒").userStatus("正常").build());
        userList.add(User.builder().userId(17L).userName("王乃醒").userStatus("正常").build());
        userDao.deleteInBatch(userList);
        //Hibernate: 
        //    delete 
        //    from
        //        t_user 
        //    where
        //        user_id=? 
        //        or user_id=?  只执行一次删除的SQL
    }
}
```



```java
	@Transactional
	public void deleteInBatch(Iterable<T> entities) {
		Assert.notNull(entities, "The given Iterable of entities not be null!");
		if (!entities.iterator().hasNext()) {
			return;
		}

		applyAndBind(getQueryString(DELETE_ALL_QUERY_STRING, entityInformation.getEntityName()), entities, em)
				.executeUpdate();
	}

	public static <T> Query applyAndBind(String queryString, Iterable<T> entities, EntityManager entityManager) {

		Assert.notNull(queryString, "Querystring must not be null!");
		Assert.notNull(entities, "Iterable of entities must not be null!");
		Assert.notNull(entityManager, "EntityManager must not be null!");

		Iterator<T> iterator = entities.iterator();

		if (!iterator.hasNext()) {
			return entityManager.createQuery(queryString);
		}

		String alias = detectAlias(queryString);
		StringBuilder builder = new StringBuilder(queryString);
		builder.append(" where");

		int i = 0;

		while (iterator.hasNext()) {

			iterator.next();

			builder.append(String.format(" %s = ?%d", alias, ++i));

			if (iterator.hasNext()) {
				builder.append(" or");  //底层用or作为删除条件拼接删除的SQL
			}
		}

		Query query = entityManager.createQuery(builder.toString());

		iterator = entities.iterator();
		i = 0;

		while (iterator.hasNext()) {
			query.setParameter(++i, iterator.next());
		}

		return query;
	}
```

# 8、deleteAll() deleteAllInBatch()

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    @Autowired
    private UserDao userDao;
    
    @Test
    public void test(){
        
        userDao.deleteAll();
        
        userDao.deleteAllInBatch();
    }
}
```

```java
	@Transactional
	public void deleteAll() {
		//deleteAll()是使用先查询全部集合，再调用delete方法逐一删除
		for (T element : findAll()) {
			delete(element);
		}
	}
```

```java
	@Transactional
	public void deleteAllInBatch() {
			//deleteAllInBatch()是通过SQL进行删除全部删除。
		em.createQuery(getDeleteAllQueryString()).executeUpdate();
	}
```

# 9、 findOne(ID id)

- 可以根据ID查询

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    
    @Autowired
    private UserDao userDao;
    
    @Test
    public void test(){
        
      User user =  userDao.findOne(4L);
        
      System.out.println(user);
    }
}
```

```java
public T findOne(ID id) {

		Assert.notNull(id, ID_MUST_NOT_BE_NULL);

		Class<T> domainType = getDomainClass();

		if (metadata == null) {
			return em.find(domainType, id);
		}

		LockModeType type = metadata.getLockModeType();

		Map<String, Object> hints = getQueryHints();
		//底层是通过实体管理器提供的find方法。
		return type == null ? em.find(domainType, id, hints) : em.find(domainType, id, type, hints);
	}
```

# 10、getOne(ID id)

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    @Autowired
    private UserDao userDao;
    
    @Test
    public void test(){
      User user =  userDao.getOne(4L);
        
        System.out.println(user);
    }
}
```

```java
//通过getOne()方法，根据ID获取。需要将POJO实体上加上@Proxy(lazy=false)注解，取消懒加载。
@Entity(name = "t_user")
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
@Proxy(lazy = false)
public class User  {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "user_id")
    private Long userId;
    @Column(name = "user_name")
    private String userName;
    @Column(name = "user_status")
    private String userStatus;
}
```

```java
	@Override
	public T getOne(ID id) {

		Assert.notNull(id, ID_MUST_NOT_BE_NULL);
        
		return em.getReference(getDomainClass(), id);
	}
```

# 11、findOne(Example example)

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    
    @Autowired
    private UserDao userDao;
   
    @Test
    public void test(){
        
        Example<User> example = Example.of(User.builder().userName("王乃醒是帅哥！").build());
      
        User user =  userDao.findOne(example);
       
        System.out.println(user);
       
        //Hibernate:
        //    select
        //        user0_.user_id as user_id1_1_,
        //        user0_.user_name as user_nam2_1_,
        //        user0_.user_status as user_sta3_1_
        //    from
        //        t_user user0_
        //    where
        //        user0_.user_name=?
    }
}
```

# 12、findAll()

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    
    @Autowired
    private UserDao userDao;
   
    @Test
    public void test(){
   
      List<User> userList =  userDao.findAll();
        
        System.out.println(userList);
    }
}
```

```java
	public List<T> findAll() {
        //底层通过Query接口，通过getReslutList()方法查询得到。
		return getQuery(null, (Sort) null).getResultList();
	}
```

# 12、findAll(Example example)

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    
    @Autowired
    private UserDao userDao;
    
    @Test
    public void test(){
        Example<User> example = Example.of(User.builder().userName("王乃醒").build());
        
        List<User> userList =  userDao.findAll(example);
      
        System.out.println(userList);
      
        //Hibernate: 
        //    select
        //        user0_.user_id as user_id1_1_,
        //        user0_.user_name as user_nam2_1_,
        //        user0_.user_status as user_sta3_1_ 
        //    from
        //        t_user user0_ 
        //    where
        //        user0_.user_name=?
        //[User(userId=2, userName=王乃醒, userStatus=正常), User(userId=8, userName=王乃醒, userStatus=正常)]
        //
        //进程已结束，退出代码为 0
    }
}
```

# 13、findAll(Sort sort)

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    
    @Autowired
    private UserDao userDao;
   
    @Test
    public void test(){
        Sort sort = new Sort(
                          new Sort.Order(Sort.Direction.DESC,"userName"),
                          new Sort.Order(Sort.Direction.ASC,"userId"));
       
        userDao.findAll(sort);
       
        //Hibernate: 
        //    select
        //        user0_.user_id as user_id1_1_,
        //        user0_.user_name as user_nam2_1_,
        //        user0_.user_status as user_sta3_1_ 
        //    from
        //        t_user user0_ 
        //    order by
        //        user0_.user_name desc,
        //        user0_.user_id asc
    }
}
```

# 14、findAll(Pageable pageable)

```java
@SpringJUnitConfig(locations = "classpath:spring-jpa.xml")
public class getBeanTest {
    @Autowired
    private UserDao userDao;
    @Test
    public void test(){
        int pageNo = 0;
        int pageSize = 3;
        Pageable pageable = new PageRequest(pageNo,pageSize);
        Page<User> page =   userDao.findAll(pageable);
       
        System.out.println("当前页：->" + page.getNumber());
        System.out.println("每页大小:->" +  page.getSize());
        System.out.println("总页码：->"+page.getTotalPages());
        System.out.println("总记录数->"+page.getTotalElements());
        System.out.println("------分页数据-------");
        page.getContent().forEach(System.out::println);
        
        //Hibernate: 
        //    select
        //        user0_.user_id as user_id1_1_,
        //        user0_.user_name as user_nam2_1_,
        //        user0_.user_status as user_sta3_1_ 
        //    from
        //        t_user user0_ limit ?
        //Hibernate: 
        //    select
        //        count(user0_.user_id) as col_0_0_ 
        //    from
        //        t_user user0_
        //当前页：->0
        //每页大小:->3
        //总页码：->4
        //总记录数->11
        //------分页数据-------
        //User(userId=1, userName=张三, userStatus=正常)
        //User(userId=2, userName=王乃醒, userStatus=正常)
        //User(userId=3, userName=java, userStatus=正常)
        //
        //进程已结束，退出代码为 0

    }
}
```


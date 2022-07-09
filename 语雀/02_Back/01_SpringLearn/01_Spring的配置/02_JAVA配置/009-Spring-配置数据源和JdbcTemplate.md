## 1、SpringConfig

```java
@Configuration
public class SpringConfig {
    @Bean
    public DataSource dataSource(){
        DriverManagerDataSource driverManagerDataSource = new DriverManagerDataSource();
        driverManagerDataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        driverManagerDataSource.setUsername("root");
        driverManagerDataSource.setPassword("root");
        driverManagerDataSource.setUrl("jdbc:mysql:///spring");
        return driverManagerDataSource;
    }
    
    @Bean
    public JdbcTemplate jdbcTemplate(DataSource dataSource){
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }
}

```

## 2、CRUD

```java
@SpringJUnitConfig(locations = "classpath:/bean.xml")
public class JdbcTemplateTest {
   	@Autowired
    private   JdbcTemplate jdbcTemplate;

    @Test
    public void testDDL(){
        String sql1 = "insert into user values(null,'张三','广西梧州')";
        String sql2 = "delete from user where id =  5";
        String sql3 =  "update user set username = '王五' where id = 3";
        jdbcTemplate.update(sql1);
        jdbcTemplate.update(sql2);
        jdbcTemplate.update(sql3);
    }

    @Test
    public void  testQuery(){
        String sql = "select * from user";
        List<User> userList = jdbcTemplate.query(sql, new BeanPropertyRowMapper<>(User.class));
        userList.forEach(System.out::println);
    }

}
```

## 3、手动处理结果集映射

```java
@SpringJUnitConfig(locations = "classpath:/bean.xml")
public class JdbcTemplateTest {
   	@Autowired
    private   JdbcTemplate jdbcTemplate;
 	
 	@Test
    public void testQuery2(){
        String sql = "select * from user";
        List<User> userList = jdbcTemplate.query(sql,(ResultSet resultSet, int i)-> {
            int id = resultSet.getInt("id");
            String username = resultSet.getString("username");
            String address = resultSet.getString("address");
            return User.builder().username(username).address(address).id(id).build();
            });
        userList.forEach(System.out::println);
    }
}
```

##  4、批量操作

```java
  @Override
    public void batchUpdate(List<Object[]> batchArgs) {
        String sql = "update t_user set user_name = ?,user_status = ? WHERE user_id = ?";
        jdbcTemplate.batchUpdate(sql,batchArgs);
    }

    @Override
    public void batchDelete(List<Object[]> batchArgs) {
        String sql = "delete from t_user where user_id = ?";
        jdbcTemplate.batchUpdate(sql,batchArgs);
    }

    @Override
    public void batchInsert(List<Object[]> batchArgs) {
        String sql = "insert into t_user(user_id,user_name,user_status) values(?,?,?)";
        jdbcTemplate.batchUpdate(sql,batchArgs);
    }
```
# SpringBoot-Merge-JPA

```yaml
spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/examinedb?useSSL=false&allowPublicKeyRetrieval=true
    username: root
    password: root
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    database: MYSQL
    show-sql: true
    open-in-view: true
    properties:
      hibernate:
        enable_lazy_load_no_trans: true #使用延时加载时控制Session的生命周期
        dialect: org.hibernate.dialect.MySQL5Dialect
        ddl-auto: update
```

```java
@Data
@Entity
@Table ( name ="t_user" , schema = "")
public class User  implements Serializable {

	/**
	 * 自增主键
	 */
	@Id
   	@Column(name = "id" )
	@GeneratedValue(strategy=GenerationType.AUTO)
	private Long id;

	/**
	 * 用户名
	 */
   	@Column(name = "username" )
	private String username;

	/**
	 * 密码
	 */
   	@Column(name = "pwd" )
	private String pwd;

	/**
	 * 角色集合
 	 */
	@ManyToMany(targetEntity = Role.class)
	@JoinTable(name = "t_user_role_relation",joinColumns = {@JoinColumn(name = "user_id")},inverseJoinColumns = {@JoinColumn(name="role_id")})
	private List<Role> roleList;
	
}
```

```java
public interface RoleDao extends CrudRepository<Role,Long> {}
```


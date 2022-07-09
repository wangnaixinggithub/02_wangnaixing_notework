# SpringBoot-Merge-JPA-Use-Entity-Anno

# 1、Need Config

## 1.1、Enum Handler

```java
public enum  Gender {

    MAIL("男性"),FMAIL("女性");

    private String value;

    private Gender(String value){
        this.value = value;
    }

}


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

    @Column(name = "name",
            unique = false,
            nullable = true,
            insertable = true,
            updatable = true,
            columnDefinition = "varchar",
            length = 250)
    private String name;

    @Column(name = "email",
            unique = false,
            insertable = true,
            updatable = true,
            columnDefinition = "varchar",
            length = 250)
    private String email;


    @Column(name = "sex",
            unique = false,
            insertable = true,
            updatable = true,
            columnDefinition = "varchar",
            length = 250)
    @Enumerated(EnumType.STRING)
    private Gender gender;


    @Column(name = "address",
            unique = false,
            insertable = true,
            updatable = true,
            columnDefinition = "varchar",
            length = 250)
    private String address;

    @Temporal(TemporalType.TIMESTAMP)
    @Column(name = "birth",
            unique = false,
            insertable = true,
            updatable = true,
            columnDefinition = "datetime")
    private Date birth;

    @Column(name = "age",
            unique = false,
            insertable = true,
            updatable = true,
            columnDefinition = "int",
            length = 11)
    private Integer age;

    @Transient
    private Integer phone;


}
```

## 1.2、@ManyToMany



```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Entity
@Table ( name ="ums_role" , schema = "")
public class UmsRole  implements Serializable {

	private static final long serialVersionUID =  5383854760300941727L;

	@Id
   	@Column(name = "id" )
	@GeneratedValue(strategy= GenerationType.IDENTITY)
	private Long id;

	/**
	 * 名称
	 */
   	@Column(name = "name" )
	private String name;

	/**
	 * 描述
	 */
   	@Column(name = "description" )
	private String description;

	/**
	 * 后台用户数量
	 */
   	@Column(name = "admin_count" )
	private Long adminCount;

	/**
	 * 创建时间
	 */
   	@Column(name = "create_time" )
	private Date createTime;

	/**
	 * 启用状态：0->禁用；1->启用
	 */
   	@Column(name = "status" )
	private Long status;

   	@Column(name = "sort" )
	private Long sort;

}


@Data
@AllArgsConstructor
@NoArgsConstructor
@Entity
@Table ( name ="ums_admin" , schema = "")
public class UmsAdmin  implements Serializable {


	@Id
   	@Column(name = "id" )
	@GeneratedValue(strategy= GenerationType.IDENTITY)
	private Long id;

   	@Column(name = "username" )
	private String username;

   	@Column(name = "password" )
	private String password;

	/**
	 * 头像
	 */
   	@Column(name = "icon" )
	private String icon;

	/**
	 * 邮箱
	 */
   	@Column(name = "email" )
	private String email;

	/**
	 * 昵称
	 */
   	@Column(name = "nick_name" )
	private String nickName;

	/**
	 * 备注信息
	 */
   	@Column(name = "note" )
	private String note;

	/**
	 * 创建时间
	 */
   	@Column(name = "create_time" )
	private Date createTime;

	/**
	 * 最后登录时间
	 */
   	@Column(name = "login_time" )
	private Date loginTime;

	/**
	 * 帐号启用状态：0->禁用；1->启用
	 */
   	@Column(name = "status" )
	private Long status;


   	@ManyToMany(cascade = CascadeType.ALL,fetch = FetchType.EAGER)
	@JoinTable(
			name = "ums_admin_role_relation",
			joinColumns = @JoinColumn(name = "admin_id",referencedColumnName = "id"),
			inverseJoinColumns = @JoinColumn(name = "role_id",referencedColumnName = "id"))
   	private List<UmsRole>  umsRole = new ArrayList<>();


}
```

## 	1.3、@ManyToOne @OneToMany 

```java
@AllArgsConstructor
@NoArgsConstructor
@Data
@Entity
@Table ( name ="ums_resource" , schema = "")
public class UmsResource  implements Serializable {

	private static final long serialVersionUID =  1980195108771238639L;

	@Id
   	@Column(name = "id" )
	private Long id;

	/**
	 * 创建时间
	 */
   	@Column(name = "create_time" )
	private Date createTime;

	/**
	 * 资源名称
	 */
   	@Column(name = "name" )
	private String name;

	/**
	 * 资源URL
	 */
   	@Column(name = "url" )
	private String url;

	/**
	 * 描述
	 */
   	@Column(name = "description" )
	private String description;


   	@ManyToOne(cascade = CascadeType.ALL,fetch = FetchType.EAGER)
	@JoinColumn(name = "category_id",referencedColumnName = "id")
   	private UmsResourceCategory resourceCategory;



}


@AllArgsConstructor
@NoArgsConstructor
@Data
@Entity
@Table ( name ="ums_resource_category" , schema = "")
public class UmsResourceCategory  implements Serializable {

	private static final long serialVersionUID =  5700842018063728170L;

	@Id
   	@Column(name = "id" )
	private Long id;

	/**
	 * 创建时间
	 */
   	@Column(name = "create_time" )
	private Date createTime;

	/**
	 * 分类名称
	 */
   	@Column(name = "name" )
	private String name;

	/**
	 * 排序
	 */
   	@Column(name = "sort" )
	private Long sort;

	@OneToMany(cascade = CascadeType.ALL,fetch = FetchType.EAGER)
   	List<UmsResource> umsResources = new ArrayList<>();


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

    //自动的封装成枚举
    @Test
    void  finAll() {
        List<User> userList = userRepository.findAll(Sort.by(new Sort.Order(Sort.Direction.DESC, "age")));
        print(userList);

    }
    //自动的  Gender.FMAIL => 女性
    @Test 
    void test2(){
        User user = new User(null, "jack11", "123456@126.com", Gender.FMAIL,"shanghai", new Date(), 12, 11);
        userRepository.save(user);
    }



}

```



```java
@SpringBootTest
class UmsAdminRepositoryTest {
    @Resource
    private UmsAdminRepository adminRepository;

    //多对多关系查询
    @Test
    void test01(){
        UmsAdmin umsAdmin = adminRepository.findById(1L).orElse(null);
        System.out.println(umsAdmin);
   
        //Hibernate: select umsadmin0_.id as id1_0_0_, umsadmin0_.create_time as create_t2_0_0_, umsadmin0_.email as email3_0_0_, umsadmin0_.icon as icon4_0_0_, umsadmin0_.login_time as login_ti5_0_0_, umsadmin0_.nick_name as nick_nam6_0_0_, umsadmin0_.note as note7_0_0_, umsadmin0_.password as password8_0_0_, umsadmin0_.status as status9_0_0_, umsadmin0_.username as usernam10_0_0_, umsrole1_.admin_id as admin_id1_1_1_, umsrole2_.id as role_id2_1_1_, umsrole2_.id as id1_5_2_, umsrole2_.admin_count as admin_co2_5_2_, umsrole2_.create_time as create_t3_5_2_, umsrole2_.description as descript4_5_2_, umsrole2_.name as name5_5_2_, umsrole2_.sort as sort6_5_2_, umsrole2_.status as status7_5_2_ 
        // from ums_admin umsadmin0_ left outer join ums_admin_role_relation umsrole1_ on umsadmin0_.id=umsrole1_.admin_id 
        // left outer join ums_role umsrole2_ on umsrole1_.role_id=umsrole2_.id 
        // where umsadmin0_.id=?
       
        //UmsAdmin(id=1, username=test, password=$2a$10$NZ5o7r2E.ayT2ZoxgjlI.eJ6OEYqjH7INR/F.mXDbjZJi9HF0YCVG, icon=http://macro-oss.oss-cn-shenzhen.aliyuncs.com/mall/images/20180607/timg.jpg, email=test@qq.com, nickName=测试账号, note=null, createTime=2018-09-29 13:55:30.0, loginTime=2018-09-29 13:55:39.0, status=1, umsRole=[UmsRole(id=1, name=商品管理员, description=只能查看及操作商品, adminCount=0, createTime=2020-02-03 16:50:37.0, status=1, sort=0), UmsRole(id=2, name=订单管理员, description=只能查看及操作订单, adminCount=0, createTime=2018-09-30 15:53:45.0, status=1, sort=0), UmsRole(id=5, name=超级管理员, description=拥有所有查看和操作功能, adminCount=0, createTime=2020-02-02 15:11:05.0, status=1, sort=0)])
    }


}

```

```java
@SpringBootTest
class UmsResourceCategoryTest {
    @Resource
    private UmsResourceCategoryRepository resourceCategoryRepository;

    //测试一对多
    @Test
    void test(){
        UmsResourceCategory category = resourceCategoryRepository.findById(1L).orElse(null);
        System.out.println(category);
        
        //Hibernate: select umsresourc0_.id as id1_3_0_, umsresourc0_.create_time as create_t2_3_0_, umsresourc0_.name as name3_3_0_, umsresourc0_.sort as sort4_3_0_, umsresourc1_.ums_resource_category_id as ums_reso1_4_1_, umsresourc2_.id as ums_reso2_4_1_, umsresourc2_.id as id1_2_2_, umsresourc2_.create_time as create_t2_2_2_, umsresourc2_.description as descript3_2_2_, umsresourc2_.name as name4_2_2_, umsresourc2_.category_id as category6_2_2_, umsresourc2_.url as url5_2_2_, umsresourc3_.id as id1_3_3_, umsresourc3_.create_time as create_t2_3_3_, umsresourc3_.name as name3_3_3_, umsresourc3_.sort as sort4_3_3_ from ums_resource_category umsresourc0_ left outer join ums_resource_category_ums_resources umsresourc1_ on umsresourc0_.id=umsresourc1_.ums_resource_category_id
        // left outer join ums_resource umsresourc2_ on umsresourc1_.ums_resources_id=umsresourc2_.id
        // left outer join ums_resource_category umsresourc3_ on umsresourc2_.category_id=umsresourc3_.id
        // where umsresourc0_.id=?

        //UmsResourceCategory(id=2, createTime=2020-02-05 10:22:34.0, name=订单模块, sort=0, umsResources=[])
    }
}

```

```java
@SpringBootTest
class UmsResourceRepositoryTest {
    @Resource
    private UmsResourceRepository resourceRepository;
    
    //测试多对一
    @Test
    void test(){
        UmsResource resource = resourceRepository.findById(1L).orElse(null);
        System.out.println(resource);
    }
}

```

```java
@SpringBootTest
class UmsResourceRepositoryTest {
    @Resource
    private UmsResourceRepository resourceRepository;

    //测试多对一
    @Test
    void test(){
        UmsResource resource = resourceRepository.findById(1L).orElse(null);
        System.out.println(resource);
        
        //Hibernate: select umsresourc0_.id as id1_2_0_, umsresourc0_.create_time as create_t2_2_0_, umsresourc0_.description as descript3_2_0_, umsresourc0_.name as name4_2_0_, umsresourc0_.category_id as category6_2_0_, umsresourc0_.url as url5_2_0_, umsresourc1_.id as id1_3_1_, umsresourc1_.create_time as create_t2_3_1_, umsresourc1_.name as name3_3_1_, umsresourc1_.sort as sort4_3_1_, umsresourc2_.ums_resource_category_id as ums_reso1_4_2_, umsresourc3_.id as ums_reso2_4_2_, umsresourc3_.id as id1_2_3_, umsresourc3_.create_time as create_t2_2_3_, umsresourc3_.description as descript3_2_3_, umsresourc3_.name as name4_2_3_, umsresourc3_.category_id as category6_2_3_, umsresourc3_.url as url5_2_3_ from ums_resource umsresourc0_ 
        // left outer join ums_resource_category umsresourc1_ on umsresourc0_.category_id=umsresourc1_.id 
        // left outer join ums_resource_category_ums_resources umsresourc2_ on umsresourc1_.id=umsresourc2_.ums_resource_category_id left outer join ums_resource umsresourc3_ on umsresourc2_.ums_resources_id=umsresourc3_.id 
        // where umsresourc0_.id=?
      
        
        //UmsResource(id=1, createTime=2020-02-04 17:04:55.0, name=商品品牌管理, url=/brand/**, description=null, resourceCategory=UmsResourceCategory(id=1, createTime=2020-02-05 10:21:44.0, name=商品模块, sort=0, umsResources=[]))
    }
}

```


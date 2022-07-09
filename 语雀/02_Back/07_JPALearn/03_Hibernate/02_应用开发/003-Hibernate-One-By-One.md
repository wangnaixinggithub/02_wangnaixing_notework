# Hibernate-One-By-One

# 1、[@OneToOne ](/OneToOne ) 

```java
//Student.java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Entity(name = "t_student")
@Builder
public class Student implements Serializable {
    @Id
    @SequenceGenerator(name = "Stu_Gen",sequenceName = "Stu_Seq")
    @GeneratedValue(strategy = GenerationType.IDENTITY,generator = "Stu_Gen")
    @Column(name = "stu_id")
    private Long stuId;

    @Column(name = "stu_no",length = 50,nullable = false,unique = true)
    private String stuNo;

    @Column(name="stu_pwd",columnDefinition = "varchar(40) not null")
    private String stuPwd;

    @Column(name ="stu_name",length = 40)
    private String stuName;
    
    @Column(name = "stu_height",columnDefinition = "decimal(8,2)")
    private Double height;

    @Column(name = "stu_weight",precision = 8,scale = 2)
    private Double weight;

    @Column(name = "entry_school_date")
    @Temporal(TemporalType.DATE)
    private Date entrySchoolDate;

    @Column(name = "create_date")
    @Temporal(TemporalType.TIMESTAMP)
    private Date createDate;

    @OneToOne
    private Clazz clazz;

}

//2. Clazz.java	
@Data
@AllArgsConstructor
@NoArgsConstructor
@Entity(name = "t_clazz")
@Builder
public class Clazz implements Serializable {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "class_id")
    private Long classId;

    @Column(name = "class_name")
    private String className;

}
```

# 2、 `entityManager.persist();`

```java
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

    @SneakyThrows
    @Test
    public void oneByOneTest() {
        //1.创建Student
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
        Date entrySchoolDate = simpleDateFormat.parse("2018-09-01");
        Student student = Student.builder()
                .stuId(null)
                .stuNo("202200801")
                .stuName("王乃醒")
                .stuPwd("root")
                .createDate(new Date())
                .height(178.3)
                .entrySchoolDate(entrySchoolDate)
                .weight(141.5)
                .build();

       //2.创建Class
        Clazz clazz = Clazz.builder()
                .classId(1L)
                .className("大数据与软件工程学院")
                .build();

        //3.Student和Class关联 一对一
        student.setClazz(clazz);

        //4.持久化操作
        entityManager.persist(clazz);
        entityManager.persist(student);
        
    }


}
```

![image-20220515152628142.png](https://cdn.nlark.com/yuque/0/2022/png/28359547/1652604648045-5849633a-ad6a-43bd-92ac-9d86d970f3b1.png#clientId=uc6166a07-1761-4&crop=0&crop=0&crop=1&crop=1&from=ui&id=K7uRq&margin=%5Bobject%20Object%5D&name=image-20220515152628142.png&originHeight=668&originWidth=466&originalType=binary&ratio=1&rotation=0&showTitle=false&size=51061&status=done&style=none&taskId=u869c8dec-5239-49de-b365-0f67b83cb73&title=)

# 3、Two [@OneToOne ](/OneToOne ) 

```java
//Student.java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Entity(name = "t_student")
@Builder
public class Student implements Serializable {
    @Id
    @SequenceGenerator(name = "Stu_Gen",sequenceName = "Stu_Seq")
    @GeneratedValue(strategy = GenerationType.IDENTITY,generator = "Stu_Gen")
    @Column(name = "stu_id")
    private Long stuId;

    @Column(name = "stu_no",length = 50,nullable = false,unique = true)
    private String stuNo;

    @Column(name="stu_pwd",columnDefinition = "varchar(40) not null")
    private String stuPwd;

    @Column(name ="stu_name",length = 40)
    private String stuName;
    
    @Column(name = "stu_height",columnDefinition = "decimal(8,2)")
    private Double height;

    @Column(name = "stu_weight",precision = 8,scale = 2)
    private Double weight;

    @Column(name = "entry_school_date")
    @Temporal(TemporalType.DATE)
    private Date entrySchoolDate;

    @Column(name = "create_date")
    @Temporal(TemporalType.TIMESTAMP)
    private Date createDate;

    @OneToOne
    private Clazz clazz;

}

//2. Clazz.java	
@Data
@AllArgsConstructor
@NoArgsConstructor
@Entity(name = "t_clazz")
@Builder
public class Clazz implements Serializable {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "class_id")
    private Long classId;

    @Column(name = "class_name")
    private String className;
    
    @OneToOne   
    private Student student;

}
```

会生成两个外键。

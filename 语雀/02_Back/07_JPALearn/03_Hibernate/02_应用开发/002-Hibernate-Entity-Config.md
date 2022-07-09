# Hibernate-Entity-Config

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Entity(name = "t_student")
public class Student implements Serializable {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
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

    @Transient
    private String aliasName;
}
```




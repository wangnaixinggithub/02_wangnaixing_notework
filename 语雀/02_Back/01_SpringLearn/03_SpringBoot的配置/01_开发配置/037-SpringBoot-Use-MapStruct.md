# SpringBoot-Use-MapStruct

# 1、Need Config

![](D:\my_notebook\干掉 BeanUtils！试试这款 Bean 自动映射工具，真心强大！\QQ截图20211121180128.png)

```xml
<dependency>
    
    <!--MapStruct相关依赖-->
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct</artifactId>
        <version>${mapstruct.version}</version>
    </dependency>
    
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct-processor</artifactId>
        <version>${mapstruct.version}</version>
        <scope>compile</scope>
    </dependency>
    
</dependencies>
```

# 2、In Your Business

```java
@Data
@EqualsAndHashCode(callSuper = false)
public class Member implements Serializable {
    /**
     * 自增主键
     */
    private Long id;
    /**
     * 用户名
     */
    private String username;
    /**
     * 密码
     */
    private String password;
    /**
     * 昵称
     */
    private String nickname;
    /**
     *生日
     */
    private Date birthday;
    /**
     *生日
     */
    private String phone;
    /**
     *生日
     */
    private String icon;
    /**
     * 性别：0->未知；1->男；2->女
     */
    private Integer gender;
}
```

- 然后再准备好会员的DTO对象`MemberDto`，我们需要将`Member`对象转换为`MemberDto`对象；

```java
@Data
@EqualsAndHashCode(callSuper = false)
public class MemberDto {
    /**
     * 自增主键
     */
    private Long id;
    /**
     * 用户名
     */
    private String username;
    /**
     * 密码
     */
    private String password;
    /**
     * 昵称
     */
    private String nickname;
    /**
     *生日
     */
    private String birthday;   //与PO类型不同的属性
    /**
     *生日
     */
    private String phoneNumber;    //与PO名称不同的属性
    /**
     *生日
     */
    private String icon;
    /**
     * 性别：0->未知；1->男；2->女
     */
    private Integer gender;
}

```

- 然后创建一个映射接口`MemberMapper`，实现同名同类型属性、不同名称属性、不同类型属性的映射；

```java

@Mapper
public interface MemberMapper {

    MemberMapper INSTANCE = Mappers.getMapper(MemberMapper.class);
    
    @Mapping(source = "phone",target = "phoneNumber")
    @Mapping(source = "birthday",target = "birthday",dateFormat = "yyyy-MM-dd")
    MemberDto toDto(Member member);

}
```

- 接下来在Controller中创建测试接口，直接通过接口中的`INSTANCE`实例调用转换方法`toDto`；

```java
@RestController
@Api(tags = "MapStructController", description = "MapStruct对象转换测试")
@RequestMapping("/mapStruct")
public class MapStructController {
    @ApiOperation(value = "基本映射")
    @GetMapping("/baseMapping")
    public CommonResult baseTest() {
        
        List<Member> memberList = LocalJsonUtil.getListFromJson("json/members.json", Member.class);
        
        MemberDto memberDto = MemberMapper.INSTANCE.toDto(memberList.get(0));
        
        return CommonResult.success(memberDto);
    }
}

```

# 3、Code Back

```java


package com.wnx.blog.mapper;

import com.wnx.blog.dto.MemberDto;
import com.wnx.blog.po.Member;
import java.text.SimpleDateFormat;

public class MemberMapperImpl implements MemberMapper {
    public MemberMapperImpl() {
    }

    public MemberDto toDto(Member member) {
        if (member == null) {
            return null;
        } else {
            MemberDto memberDto = new MemberDto();
            memberDto.setPhoneNumber(member.getPhone());
            if (member.getBirthday() != null) {
                memberDto.setBirthday((new SimpleDateFormat("yyyy-MM-dd")).format(member.getBirthday()));
            }

            memberDto.setId(member.getId());
            memberDto.setUsername(member.getUsername());
            memberDto.setPassword(member.getPassword());
            memberDto.setNickname(member.getNickname());
            memberDto.setIcon(member.getIcon());
            memberDto.setGender(member.getGender());
            return memberDto;
        }
    }
}

```




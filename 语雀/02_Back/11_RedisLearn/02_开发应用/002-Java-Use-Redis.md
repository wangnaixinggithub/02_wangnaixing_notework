# Java-Use-Redis

```xml
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>4.2.3</version>
        </dependency>
```

# 1、Operation String

```java
    @Test
    public void test_String() {
        
        Jedis jedis = new Jedis("http://127.0.0.1:6379");

        jedis.set("name","wangnaixing");     // Set

        System.out.println(jedis.get("name"));   // GET

        System.out.println(jedis.type("name"));  //  Type is String


    }
```

# 2、Operation  List

```java
  @Test
    public void test_List() {
        Jedis jedis = new Jedis("http://127.0.0.1:6379");

        jedis.lpush("my-list","1","2","3","4","5"); // SET

        System.out.println(jedis.lrange("my-list",0,-1));        // All element

        System.out.println(jedis.lindex("my-list",0));   // findByIndex

        System.out.println(jedis.llen("my-list"));       //  length

        System.out.println(jedis.type("my-list"));       //  type is list
    }
```

# 3、Operation Set

```java
 @Test
    public void test_Set() {
          Jedis jedis = new Jedis("http://localhost:6379");

          jedis.sadd("my-set","1","2","3","4","5");    //  Set

          System.out.println(jedis.scard("my-set"));         // 获取成员数

          System.out.println(jedis.sismember("my-set","5"));   // isMember?

          System.out.println(jedis.smembers("my-set"));        //   getAllMember

          System.out.println(jedis.type("my-set"));     // Type is set

    }
```

# 4、Operation Hash

```java
 @Test
    public void test_Hash() {
        Jedis jedis = new Jedis("http://localhost:6379");
        Map<String,String> mapping = new HashMap<>();

        jedis.hset("user","id","1");        //  SET
        jedis.hset("user","username","wangnaixing");
        jedis.hset("user","sex","Fale");
        jedis.hset("user","tel","18154622909");

        mapping.put("id","1");
        mapping.put("username","wangnaixing");
        mapping.put("sex","Fale");
        mapping.put("tel","18154622909");
        jedis.hmset("user2", mapping);

        System.out.println(jedis.hgetAll("user"));     // getAllEntry

        System.out.println(jedis.hkeys("user"));           // getAllKey

        System.out.println(jedis.hvals("user"));    // getAllValue

        System.out.println(jedis.hmget("user","id","username","sex","tel"));         // getValueByKey


    }
```
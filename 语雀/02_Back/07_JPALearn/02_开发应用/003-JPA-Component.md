# JPA-Component

# 1、Repository

```java
public interface Repository<T, ID extends Serializable> {}

public interface CrudRepository<T, ID extends Serializable> extends Repository<T, ID> {}

public interface PagingAndSortingRepository<T, ID extends Serializable> extends CrudRepository<T, ID> {}

public interface JpaRepository<T, ID extends Serializable> extends PagingAndSortingRepository<T, ID>, QueryByExampleExecutor<T> {}

public class SimpleJpaRepository<T, ID extends Serializable> implements JpaRepository<T, ID>, JpaSpecificationExecutor<T> {

 //YOUR JpaRepository
 public interface UserDao  extends JpaRepository<User,Long> { }  
```

# 2、Sort

```java
public class Sort implements Iterable<org.springframework.data.domain.Sort.Order>, Serializable {
	public static final Direction DEFAULT_DIRECTION = Direction.ASC;
	private final List<Order> orders
        
 	 public static class Order implements Serializable {}
  	public static enum Direction { ASC, DESC;｝
 }
```

# 3、Pageable

```java
public interface Pageable {}
public abstract class AbstractPageRequest implements Pageable, Serializable {}
public class PageRequest extends AbstractPageRequest {}
```

# 4、Page

```java
public interface Page<T> extends Slice<T> {}
```



# 5、QueryByExampleExecutor

```java
public interface QueryByExampleExecutor<T> {}
```

```java
public class Example<T> {
 	private final @NonNull T probe;
	private final @NonNull ExampleMatcher matcher;
}
```

# 6、ExampleMatcher

```java
public class ExampleMatcher {}
```

```java
abstract class Chunk<T> implements Slice<T>, Serializable {}
```

# 7、EntityManager

```java
public interface EntityManager {}
```

```java
public interface TypedQuery<X> extends Query {}
```

```java
public interface CriteriaBuilder {}
```


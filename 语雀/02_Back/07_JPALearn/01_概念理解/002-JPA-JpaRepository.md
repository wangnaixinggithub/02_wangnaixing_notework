# JpaRepository

# 1、JpaRepository

```java
public interface JpaRepository<T, ID extends Serializable> extends PagingAndSortingRepository<T, ID>, QueryByExampleExecutor<T> {
		
}
```

# 2、QueryByExampleExecutor

```java
public interface QueryByExampleExecutor<T> {
    //根据Example的条件查询得到一条记录
	<S extends T> S findOne(Example<S> example);
    
    //根据Example的条件查询过滤
   	<S extends T> Iterable<S> findAll(Example<S> example); 
    
    //过滤之外还支持排序
    <S extends T> Iterable<S> findAll(Example<S> example, Sort sort);
    
    //还支持分页
    <S extends T> Page<S> findAll(Example<S> example, Pageable pageable);
    
    //查数量的情况的过滤
    <S extends T> long count(Example<S> example);
    
    //是否存在？
    <S extends T> boolean exists(Example<S> example);
    
}
```

# 3、Example

```java
public class Example<T> {
	public static <T> Example<T> of(T probe) {
		return new Example<T>(probe, ExampleMatcher.matching());
	}
	public static <T> Example<T> of(T probe, ExampleMatcher matcher) {
		return new Example<T>(probe, matcher);
	}
｝
```

# 4、ExampleMatcher

```java
public class ExampleMatcher {
    
    //空值处理器
    NullHandler nullHandler;
    
   	//String类型匹配器
	StringMatcher defaultStringMatcher;
	
    //点路径定义的属性处理
	PropertySpecifiers propertySpecifiers;
	
    //忽略路径
	Set<String> ignoredPaths;
	
    //默认忽略大小写
	boolean defaultIgnoreCase;
	
    //匹配模式
	@Wither(AccessLevel.PRIVATE) MatchMode mode;
	
    //1.空值处理器
    public static enum NullHandler {INCLUDE, IGNORE}
    
    //2.String值的匹配处理器
    public static enum StringMatcher {DEFAULT,EXACT,STARTING,ENDING,CONTAINING,REGEX;}
    
    //3.为点路径定义特定的属性处理。
    public static class PropertySpecifiers {}
    
    //4.匹配类型    
	private static enum MatchMode {ALL, ANY;}
｝
```

```java
public class ExampleMatcher {
   	//私有构造
    private ExampleMatcher() {
	this(NullHandler.IGNORE, StringMatcher.DEFAULT, new PropertySpecifiers(), Collections.<String>emptySet(), false,MatchMode.ALL);
        }
    //匹配类型
    public static ExampleMatcher matching() {return matchingAll();}
    public static ExampleMatcher matchingAll() {return new ExampleMatcher().withMode(MatchMode.ALL);}
    public static ExampleMatcher matchingAny() {return new ExampleMatcher().withMode(MatchMode.ANY);}
    
    //空值处理器自定义
   public ExampleMatcher withIncludeNullValues() {return withNullHandler(NullHandler.INCLUDE);}
   public ExampleMatcher withIgnoreNullValues() {return withNullHandler(NullHandler.IGNORE);}
   public ExampleMatcher withNullHandler(NullHandler nullHandler) {}
    
   //String值匹配器自定义
   public ExampleMatcher withStringMatcher(StringMatcher defaultStringMatcher) {}
    
   // 点路径定义的属性匹配自定义
   public ExampleMatcher withIgnoreCase(String... propertyPaths) {}
    
    //忽略路径自定义
	public ExampleMatcher withIgnorePaths(String... ignoredPaths) {}
    
    //默认忽略大小写自定义
	public ExampleMatcher withIgnoreCase() {return withIgnoreCase(true);}
    
    public ExampleMatcher withIgnoreCase(boolean defaultIgnoreCase) {}
    
    
 }
```


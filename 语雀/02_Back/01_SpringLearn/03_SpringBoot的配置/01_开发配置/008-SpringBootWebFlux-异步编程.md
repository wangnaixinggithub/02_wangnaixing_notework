# SpringWebFlux-异步非阻塞编程

# 1、逻辑

```java
@Service
public class UserServiceImpl  extends ServiceImpl<UserMapper, User> implements UserService {

    @Override
    public Mono<User> findById(Integer id) {
        return Mono.justOrEmpty(baseMapper.selectById(id));
    }

    @Override
    public Flux<User> findAll() {
        return Flux.fromIterable(baseMapper.selectList(null));
    }

    @Override
    public Mono<Void> insert(Mono<User> userMono) {
        return userMono.doOnNext(item -> baseMapper.insert(item)).then(Mono.empty());
    }
}
```

```java
public class UserHandler {

    public UserHandler(UserService userService) {
        this.userService = userService;
    }

    private final UserService userService;


    public Mono<ServerResponse> findById(ServerRequest serverRequest){
        Integer id = Integer.valueOf(serverRequest.pathVariable("id"));
        Mono<User> userMono = userService.findById(id);
        return ServerResponse.ok().contentType(MediaType.APPLICATION_JSON).body(userMono,User.class);
    }

    public Mono<ServerResponse> insertUser(ServerRequest serverRequest){
        Mono<User> userMono = serverRequest.bodyToMono(User.class);
        Mono<Void> mono = userService.insert(userMono);
        return ServerResponse.ok().build(mono);
    }


    public Mono<ServerResponse> findAll(ServerRequest serverRequest) {
        Flux<User> userFlux = userService.findAll();
        return ServerResponse.ok().contentType(MediaType.APPLICATION_JSON).body(userFlux,User.class);
    }
}
```

# 2、Server

```java
public class Server {

 
}
```

# 3、RouterFunction

```java
public class Server {
    //1.创建路由
    public RouterFunction<ServerResponse> routerFunction(){
        //1.1创建处理器
        UserService userService = new UserServiceImpl();
        UserHandler handler =  new UserHandler(userService);
		
        //1.2完成处理器方法与URL的映射，返回路由功能对象。
        return RouterFunctions.route(
                GET("/user/findById/{id}").and(accept(MediaType.APPLICATION_JSON)),handler::findById)
                .andRoute(GET("/user/findAll").and(accept(MediaType.APPLICATION_JSON)),handler::findAll)
                .andRoute(POST("/user/insert").and(accept(MediaType.APPLICATION_JSON)),handler::insertUser);
    }
}
```

# 4、ReactorHttpHandlerAdapter

```java
public class Server {
    public RouterFunction<ServerResponse> routerFunction(){
        UserService userService = new UserServiceImpl();
        UserHandler handler =  new UserHandler(userService);
        return RouterFunctions.route(
                GET("/user/findById/{id}").and(accept(MediaType.APPLICATION_JSON)),handler::findById)
                .andRoute(GET("/user/findAll").and(accept(MediaType.APPLICATION_JSON)),handler::findAll)
                .andRoute(POST("/user/insert").and(accept(MediaType.APPLICATION_JSON)),handler::insertUser);
    }
    //2.创建适配器
    public void reactorHttpHandlerAdapter(){
    	//1.获取路由
        RouterFunction<ServerResponse> routerFunction = routerFunction();
        
        //2.得到路由处理器
        HttpHandler httpHandler = toHttpHandler(routerFunction);
      
        //3.对路由处理器进行适配
        ReactorHttpHandlerAdapter reactorHttpHandlerAdapter = new ReactorHttpHandlerAdapter(httpHandler);
      
        //创建HTTP服务
        HttpServer httpServer = HttpServer.create();
    
        //HTTP服务使用适配器
        httpServer.handle(reactorHttpHandlerAdapter).bindNow();

    }
```

# 5、启动Server

```java
public class Server {
   
    public RouterFunction<ServerResponse> routerFunction(){
        UserService userService = new UserServiceImpl();
        UserHandler handler =  new UserHandler(userService);

        return RouterFunctions.route(
                GET("/user/findById/{id}").and(accept(MediaType.APPLICATION_JSON)),handler::findById)
                .andRoute(GET("/user/findAll").and(accept(MediaType.APPLICATION_JSON)),handler::findAll)
                .andRoute(POST("/user/insert").and(accept(MediaType.APPLICATION_JSON)),handler::insertUser);
    }
    
    public void reactorHttpHandlerAdapter(){
        RouterFunction<ServerResponse> routerFunction = routerFunction();
        HttpHandler httpHandler = toHttpHandler(routerFunction);
        ReactorHttpHandlerAdapter reactorHttpHandlerAdapter = new ReactorHttpHandlerAdapter(httpHandler);
        HttpServer httpServer = HttpServer.create();
        httpServer.handle(reactorHttpHandlerAdapter).bindNow();

    } 
    
    //3.启动服务器
    @SneakyThrows
    public static void main(String[] args) {
        Server server = new Server();
        server.reactorHttpHandlerAdapter();
        System.out.println("启动服务器");
        System.in.read();
    }
}
```


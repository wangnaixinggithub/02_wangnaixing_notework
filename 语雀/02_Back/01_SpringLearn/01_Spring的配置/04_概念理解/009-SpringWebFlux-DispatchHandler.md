

# SpringWebFlux-DispatchHandler

- BIO：一个线程启动一个输入流，开启一个套接字。

![](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/098-%E6%9D%A8%E5%8D%9A%E8%B6%85-SpringWebFlux%E7%9A%84%E6%A0%B8%E5%BF%83API.assets/image-20220326101358613.png)

- NIO:多个线程和多个频道通过Selector选择器，匹配同时运作。

![](../../../../../spring-learn/%E7%8E%8B%E4%B9%83%E9%86%92%E7%9A%84%E7%AC%94%E8%AE%B0/098-%E6%9D%A8%E5%8D%9A%E8%B6%85-SpringWebFlux%E7%9A%84%E6%A0%B8%E5%BF%83API.assets/image-20220326101654341.png)

# DispatchHandler

```java
public class DispatcherHandler implements WebHandler, PreFlightRequestHandler, ApplicationContextAware {

	@Nullable
	private List<HandlerMapping> handlerMappings; //处理器映射器

	@Nullable
	private List<HandlerAdapter> handlerAdapters;//处理器适配器

	@Nullable
   private List<HandlerResultHandler> resultHandlers; //处理结果处理器
}
```

- 每一个HTTP请求都会执行handle()方法，携带进来一个有包含当前请求信息的ServerWebExchange服务Web交换机参数，处理请求。

```java
	@Override
	public Mono<Void> handle(ServerWebExchange exchange) {
        //1.有没有URL和Mapper方法映射的映射器？
		if (this.handlerMappings == null) {
			return createNotFoundError();
		}
        //2.跨域处理
		if (CorsUtils.isPreFlightRequest(exchange.getRequest())) {
			return handlePreFlight(exchange);
		}
        
		return Flux.fromIterable(this.handlerMappings)
				.concatMap(mapping -> mapping.getHandler(exchange)) //根据URL获取对应处理器映射器handlerMapping
				.next()
				.switchIfEmpty(createNotFoundError())
				.flatMap(handler -> invokeHandler(exchange, handler)) //调用Handler业务方法处理
				.flatMap(result -> handleResult(exchange, result)); //对Handler方法的返回值进行处理
	}
```


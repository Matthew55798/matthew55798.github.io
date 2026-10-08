# Netty 与 WebFlux

> 本篇回答三个问题：Netty 和 WebFlux 分别是什么、各自的核心原理、在真实项目里怎么选和怎么答。

## 1. 先分清层次：它们不是同一层的东西

| 维度 | Netty | WebFlux |
|------|-------|---------|
| 定位 | 网络编程框架 / NIO 客户端-服务端框架 | Spring 的响应式 Web 框架 |
| 层次 | 传输层之上，负责协议编解码（TCP/UDP/HTTP/WebSocket） | Web 应用层，负责路由、参数绑定、编解码、异常处理 |
| 编程模型 | EventLoop + Channel + Pipeline | Reactive Streams（`Mono`/`Flux`）+ 函数式或注解式端点 |
| 抽象单位 | `ByteBuf`、`ChannelHandler` | `Mono<T>`、`Flux<T>`、`ServerWebExchange` |
| 默认运行时 | 自己就是运行时 | 默认跑在 Reactor Netty（也就是 Netty）上 |

一句话概括：**Netty 是「怎么收发字节」的框架，WebFlux 是「怎么把请求映射成业务流」的框架**。WebFlux 默认的 HTTP 服务器就是 Netty，所以两者经常一起出现，但它们是可以各自替换的两层。

## 2. Netty 核心：一个主从 Reactor 多线程模型

Netty 解决的问题是：JDK 原生 NIO 的 `Selector`、`ByteBuffer` API 太底层，业务方要自己处理半包粘包、编解码、连接管理、线程调度和内存泄漏。

### 2.1 线程模型

```
BossGroup（1 个 EventLoop）        WorkerGroup（N 个 EventLoop，默认 2 * CPU）
        │ accept()                          │ read / write
        └──► 注册到某个 EventLoop ───────────┘
                                            │
                                    ChannelPipeline（ChannelHandler 链）
                              decode → 业务 handler → encode → write
```

- 一个 `Channel` 一生只绑定一个 `EventLoop`；**一个 `EventLoop` 串行处理多个 Channel**，所以 handler 内部天然无锁，不需要额外同步。
- 由此推出最重要的纪律：**绝对不能在 EventLoop 里做阻塞操作**（JDBC、同步 HTTP、`Thread.sleep`）。一旦阻塞，会拖死它管理的所有连接。正确处理方式是丢给业务线程池，或者直接使用异步客户端。

### 2.2 关键组件

- **`ByteBuf`**：读写双指针（`readerIndex` / `writerIndex`）的可扩容缓冲区，支持池化和引用计数，比 `ByteBuffer` 好用得多。
- **`ChannelHandler` + `ChannelPipeline`**：责任链模型，入站和出站 handler 分别处理读写方向的事件。
- **编解码**：`ByteToMessageDecoder` / `MessageToByteEncoder` 是自定义协议的基础扩展点。
- **粘包 / 半包**：TCP 是字节流，没有消息边界，必须由应用层解决。常见方案有三种：
  - 定长：`FixedLengthFrameDecoder`
  - 分隔符：`DelimiterBasedFrameDecoder`
  - 长度字段：`LengthFieldBasedFrameDecoder`（最常用，绝大部分自定义协议都用这种）
- **零拷贝**：`CompositeByteBuf`、`FileRegion`、`slice` / `duplicate`，减少数据在用户态和内核态之间的搬运。
- **背压**：`Channel.isWritable()` 配合写水位线 `WRITE_BUFFER_WATER_MARK`，防止慢消费者把内存撑爆。
- **空闲检测**：`IdleStateHandler` 实现心跳，配合 `userEventTriggered` 处理超时断连。

### 2.3 什么时候用 Netty

需要自定义协议、长连接、十万级并发连接、IM、RPC 底层、网关底层时使用。典型用户：Dubbo、RocketMQ、gRPC-Java、Elasticsearch，以及 Spring Cloud Gateway 的底层运行时（Reactor Netty）。

## 3. WebFlux 核心：非阻塞 + 背压的端到端链路

### 3.1 为什么需要它

Spring MVC 是 thread-per-request，一个请求占一个 Tomcat 线程，**线程数就是并发上限**。当业务大部分时间在等待下游 I/O 时，线程全在阻塞等待，CPU 闲置但吞吐上不去。

WebFlux 用少量事件循环线程 + 流式组合，把「等 I/O」的时间交还给线程，从而在 I/O 密集场景下大幅提升单机并发能力。

### 3.2 两种编程模型

```java
// 1. 注解式：写法接近 MVC，但返回值必须是 Publisher，且全链路不能断
@GetMapping("/user/{id}")
public Mono<User> get(@PathVariable Long id) {
    return userService.findById(id);
}
```

```java
// 2. 函数式端点（RouterFunction）：路由表与处理逻辑分离
RouterFunction<ServerResponse> route = route()
    .GET("/user/{id}", accept(APPLICATION_JSON), req ->
        ok().body(userService.findById(req.pathVariable("id", Long.class)), User.class))
    .build();
```

### 3.3 必须记住的三个概念

1. **Reactive Streams 的三个信号**：`onNext` / `onError` / `onComplete`。对应两种发布者：`Mono`（0—1 个元素）和 `Flux`（0—N 个元素）。
2. **背压（backpressure）**：消费者通过 `request(n)` 告诉生产者「我还能接 n 个」。这是 WebFlux 相比早期纯回调方案（如 `CompletableFuture`、Servlet 异步）最本质的差别。
3. **冷热流与惰性求值**：`Mono`/`Flux` 是懒的，**没有订阅什么都不会发生**。这是排查「我的代码没执行」类问题的第一反应。

### 3.4 常用算子（面试高频）

- 转换：`map`（同步转换）、`flatMap`（异步转换 + 合并，并发度默认 256）、`concatMap`（异步但保序）
- 聚合：`zip` / `zipWith`（并发聚合）、`merge`、`then`
- 空值处理：`switchIfEmpty`、`defaultIfEmpty`
- 错误与超时：`timeout`、`retryWhen`、`onErrorResume`、`onErrorReturn`
- 副作用：`doOnNext`、`doOnError`

最常见的考法是区分 **`map` vs `flatMap`**、**`flatMap` vs `concatMap`（有序性差异）**。

### 3.5 阻塞操作怎么处理

默认运行时是 Reactor Netty，业务侧只有少数几个事件循环线程，所以阻塞调用必须显式切换线程池：

```java
public Mono<User> find(Long id) {
    return Mono.fromCallable(() -> jdbcTemplate.queryForObject(sql, User.class)) // 阻塞调用
               .subscribeOn(Schedulers.boundedElastic());                       // 切到弹性线程池
}
```

同理，`ThreadLocal` 类方案在响应式链路里会失效，`SecurityContext`、MDC 日志、事务上下文都需要专门的处理方式（`contextWrite`、`ReactorContext`）。

### 3.6 优缺点与适用场景

**优点**：I/O 密集、高并发、长连接、流式推送（SSE）、网关转发，且上下游都是异步客户端时，资源利用率和吞吐明显优于 MVC，整条链路有统一背压。

**代价**：调试困难（堆栈断裂）、学习曲线陡、阻塞驱动（JDBC / MyBatis）和 ThreadLocal 类方案需要额外适配、CPU 密集任务没有收益。

## 4. 两者的关系与选型

- **WebFlux ≠ Netty**，但 WebFlux 默认用 Netty。引入 `spring-boot-starter-webflux` 会自动带入 `reactor-netty`。
- **WebFlux 之上的产品**：Spring Cloud Gateway 就是 WebFlux + Reactor Netty 写的。所以在 Gateway 里写自定义逻辑必须用 `GatewayFilter`，而不是 Servlet `Filter`，且不能阻塞。
- **选型口诀**：
  - 需要自定义协议、长连接 → Netty
  - 标准 HTTP 服务且 I/O 密集 → WebFlux
  - 链路里有一环是阻塞驱动（MyBatis / JDBC）→ 老实上 MVC，或者只把网关、转发层做成 WebFlux
- **新变量：JDK 21 虚拟线程**。虚拟线程让「MVC + 虚拟线程」在阻塞驱动下也能拿到不错的并发，一部分原来必须上 WebFlux 的场景可以用它替代。但**有背压需求的流式场景，虚拟线程无法替代响应式**。

## 5. 结合项目的答法

把「IO / Netty / WebFlux」这行技能挂到真实项目上，面试时按下面三条组织：

1. **网关层**：项目用 `Spring Cloud Gateway` 统一鉴权。这里可以讲它是 WebFlux + Reactor Netty，全局鉴权过滤器为什么必须非阻塞，如何用 `Mono` 组合鉴权与转发，限流怎么做（`RequestRateLimiter` + Redis）。
2. **长连接**：智慧城市大屏用 `WebSocket` 与数字孪生联动。这里可以讲长连接下 thread-per-request 的瓶颈、Netty / Reactor Netty 的事件循环模型，以及为什么很多框架选择 Spring WebSocket 而不是裸 Netty（学习成本与场景覆盖率）。
3. **自研消息中间件**：这是最能体现「真的写过」的部分。可以讲 Netty 主从 Reactor、粘包半包处理、心跳与空闲检测（`IdleStateHandler`）、背压水位线。

## 6. 参考资料

- 在线：yudao-cloud 官方文档《WebSocket 实时通信》（<https://cloud.iocoder.cn/>），含「为什么不使用 Netty 实现 WebSocket」的官方问答
- 本地：`软件工程/技术/常用框架/Spring.md`（Web 模块与 `spring-webflux` 定位）

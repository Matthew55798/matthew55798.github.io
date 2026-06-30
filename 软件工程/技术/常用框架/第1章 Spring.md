# 第1章 Spring

## 什么是 Spring Framework？

我们一般说 Spring 框架指的都是 Spring Framework，它是很多模块的集合，使用这些模块可以很方便地协助我们进行开发，比如说 Spring 支持 IoC（Inversion of Control:控制反转） 和 AOP(Aspect-Oriented Programming:面向切面编程)、可以很方便地对数据库进行访问、可以很方便地集成第三方组件（电子邮件，任务，调度，缓存等等）、对单元测试支持比较好、支持 RESTful Java 应用程序的开发。

- **IoC（Inversion of Control，控制反转）**：把对象创建、依赖关系维护和生命周期管理交给 Spring 容器，而不是在业务代码中手动 `new` 对象。这样可以降低组件之间的耦合度，也方便统一管理单例、配置、依赖替换和测试替身。
- **DI（Dependency Injection，依赖注入）**：IoC 的一种具体实现方式。对象不再主动查找依赖，而是由容器在创建对象时把依赖注入进来。
- **AOP（Aspect-Oriented Programming，面向切面编程）**：把日志、事务、权限、监控、缓存等横切逻辑从业务代码中抽离出来，通过代理机制织入到目标方法前后，减少重复代码并提升可维护性。

几个名称的含义如下：

- **Spring Framework**：基础框架项目，包含 IoC 容器、AOP、事务、数据访问、Web、测试等模块。
- **Spring MVC**：Spring Framework 中的 Servlet Web MVC 框架，对应 `spring-webmvc` 模块，围绕 `DispatcherServlet` 处理 Web 请求。

## Spring Framework 包含哪些模块？

从 Spring Framework 官方参考文档的组织方式看，核心内容主要分为 Core Technologies、Testing、Data Access、Web Servlet、Web Reactive、Integration、Languages 等部分。结合 JavaGuide 常见面试题的传统归类，可以按下面几类理解。

### Core Container

Core Container 是 Spring Framework 的基础，主要包括 `spring-core`、`spring-beans`、`spring-context`、`spring-context-support`、`spring-expression` 等模块。

- **spring-core**：提供框架最底层的工具类和基础设施，比如资源加载、类型转换、反射工具等。
- **spring-beans**：提供 BeanFactory、BeanDefinition 等 Bean 管理能力，是 IoC 容器的基础。
- **spring-context**：建立在 core 和 beans 之上，提供 ApplicationContext、事件发布、国际化、环境抽象、注解配置等更完整的应用上下文能力。
- **spring-context-support**：提供对第三方库的集成支持，例如缓存、邮件、模板引擎、任务调度等常见应用服务。
- **spring-expression**：提供 SpEL（Spring Expression Language），用于在配置、注解和运行时表达式中访问对象属性、调用方法、进行条件判断等。

一般说 Spring 的“容器”，主要就是指这一组模块提供的 IoC/DI 能力。

### AOP、Aspects 与 Instrumentation

这部分模块用于处理横切关注点和类增强。

- **spring-aop**：提供 Spring AOP 能力，通常基于 JDK 动态代理或 CGLIB 代理实现方法级增强。事务、缓存、权限校验、日志记录等都可以基于 AOP 模型实现。
- **spring-aspects**：提供与 AspectJ 的集成，适合需要更完整切面能力的场景。
- **spring-instrument**：提供类加载器级别的 instrumentation 支持，常用于特定应用服务器或高级增强场景。

Spring AOP 更偏向轻量级、运行时代理，覆盖大多数企业应用常见需求；AspectJ 则能力更完整，但引入和理解成本更高。

### Data Access 与 Integration

Data Access/Integration 用于简化数据库访问、事务处理、对象关系映射和消息集成。

- **spring-jdbc**：封装 JDBC 常见样板代码，简化连接获取、SQL 执行、异常转换和资源释放。
- **spring-tx**：提供声明式和编程式事务管理，是 `@Transactional` 的基础。
- **spring-orm**：集成 JPA、Hibernate 等 ORM 技术，让 ORM 框架可以纳入 Spring 的事务和容器管理。
- **spring-oxm**：提供对象与 XML 之间的映射抽象。
- **spring-jms**：简化 JMS 消息发送、接收和监听器配置。

这一组模块主要处理资源管理、事务管理和异常转换。例如不同数据库访问技术抛出的底层异常可以被转换为 Spring 的 DataAccessException 体系，事务也可以通过统一的事务抽象来管理。

### Web

Web 模块用于构建 Web 应用、REST API、WebSocket 应用，以及响应式 Web 应用。

- **spring-web**：提供 Web 基础能力，例如 multipart 文件上传、Web 应用上下文、HTTP 客户端和远程调用基础设施。
- **spring-webmvc**：也就是 Spring MVC，基于 Servlet API，提供 DispatcherServlet、Controller、HandlerMapping、ViewResolver、数据绑定、校验和 REST 支持。
- **spring-websocket**：提供 WebSocket 和 STOMP 消息支持。
- **spring-webflux**：响应式 Web 框架，基于 Reactive Streams，适合非阻塞 I/O 和响应式编程模型。

多数传统 Java Web 项目使用的是 Spring MVC；如果项目基于响应式链路、需要非阻塞端到端模型，才会考虑 Spring WebFlux。

### Testing

`spring-test` 提供 Spring 应用测试支持，重点包括：

- TestContext Framework：在测试中加载和缓存 Spring ApplicationContext。
- Mock 对象：提供 Servlet API、Web 环境等测试替身。
- Spring MVC Test：不用启动完整服务器也能测试 MVC Controller。
- WebTestClient：可用于测试 WebFlux，也可测试 Servlet Web 应用。

这部分能力让单元测试、集成测试和 Web 层测试能够更自然地接入 Spring 容器。

### Integration 与其他能力

Spring Framework 还提供很多面向工程集成的能力，例如缓存抽象、任务调度、邮件发送、JMX、JCA、应用事件、资源管理、校验、数据绑定和可观测性接入等。它们不一定都作为面试题中的“大模块”出现，但在真实项目中经常作为基础设施被使用。

## Spring 和 Spring MVC 的关系

### Spring Framework 是基础

Spring Framework 提供 Java 应用的基础编程模型和基础设施。IoC 容器负责管理对象，AOP 负责处理横切逻辑，事务抽象负责统一事务管理，数据访问模块负责简化持久化集成，Web 模块负责支持 Web 开发。

Spring MVC 属于 Spring Framework 的 Web MVC 部分，二者不是平级关系。

### Spring MVC 是 Spring Framework 的 Web MVC 框架

Spring MVC 是 Spring Framework 的一个 Web 模块。它围绕 `DispatcherServlet` 构建，请求进入后由前端控制器统一分发，再通过 HandlerMapping 找到 Controller，经过参数绑定、校验、业务调用和结果处理后返回响应。

因此，Spring MVC 不是和 Spring Framework 平级的项目，而是 Spring Framework 中基于 Servlet API 的 Web MVC 框架。

## Spring 创建 Bean 的过程

### Spring 创建 Bean 的流程

Spring 创建一个单例 Bean 时，可以按下面的主流程理解：

1. 获取 BeanDefinition：Spring 先根据 Bean 名称找到对应的 BeanDefinition，里面保存了 Bean 的类型、作用域、构造参数、属性依赖、初始化方法等元数据。
2. 实例化 Bean：根据构造方法或工厂方法创建一个原始对象。此时对象已经被创建出来，但依赖属性还没有完成注入。
3. 提前暴露单例对象：如果这是单例 Bean，并且允许循环依赖，Spring 会在属性注入前把一个可以获取早期引用的工厂对象放入三级缓存。
4. 属性填充：Spring 根据 `@Autowired`、`@Resource`、XML 或 Java 配置等信息给 Bean 注入依赖。如果依赖的 Bean 还没创建，就会触发依赖 Bean 的创建。
5. 初始化：执行初始化相关逻辑，常见包括初始化方法、初始化注解，以及 AOP 代理创建等扩展逻辑。
6. 放入单例池：Bean 创建完成后，Spring 将最终对象放入一级缓存，后续再获取这个 Bean 时直接从单例池返回。

结合生命周期记忆，可以把 Bean 创建过程简化成：**实例化 -> 属性注入 -> 初始化 -> 放入单例池**。循环依赖问题主要发生在“实例化之后、属性注入之前”这个阶段。

### 循环依赖了解吗？怎么解决？

循环依赖指两个或多个 Bean 之间相互依赖，例如 A 依赖 B，B 又依赖 A。

Spring 对单例 Bean 的部分循环依赖有处理机制，核心是提前暴露尚未完成初始化的 Bean 引用。相关缓存包括：

- **一级缓存 `singletonObjects`**：保存已经初始化完成的单例 Bean。
- **二级缓存 `earlySingletonObjects`**：保存提前暴露的 Bean 早期引用。
- **三级缓存 `singletonFactories`**：保存可以生成 Bean 早期引用的 `ObjectFactory`。

以 A 依赖 B、B 又依赖 A 为例：Spring 创建 A 时，会先实例化 A，并把可以生成 A 早期引用的 `ObjectFactory` 放入三级缓存；随后给 A 注入 B，触发 B 的创建；B 注入 A 时，可以通过三级缓存拿到 A 的早期引用，从而完成依赖注入。

三级缓存的重点在于 AOP 场景：提前暴露的对象可能需要是代理对象，而不是原始对象。

并不是所有循环依赖都能被 Spring 解决。构造器注入导致的循环依赖、原型 Bean 循环依赖，以及部分代理或异步场景下的循环依赖，仍然可能创建失败。实际业务代码中也不应该依赖循环依赖机制，出现循环依赖通常说明对象职责或依赖方向需要重新设计。

## Spring IoC

### 什么是 IoC？

IoC（Inversion of Control，控制反转）是一种设计思想，描述的是对象创建、依赖装配和生命周期管理的控制权从业务代码转移到外部容器。

没有 IoC 时，一个对象往往需要自己通过 `new` 创建依赖对象；使用 Spring 后，对象只声明自己需要什么依赖，具体创建和注入由 Spring IoC 容器负责。

这里的“控制”指对象创建和管理的权力，“反转”指这部分权力交给了外部容器。

### IoC 解决了什么问题？

IoC 主要解决对象之间强耦合的问题。

如果 Service 层直接 `new` 某个 DAO 实现类，当 DAO 实现切换时，所有依赖该实现类的位置都可能要修改。使用 IoC 后，业务类依赖接口或抽象，具体实现交给容器装配，替换实现类时通常只需要调整 Bean 定义或配置。

IoC 还让对象更容易被统一管理，例如单例对象、配置属性、事务代理、测试替身等都可以由容器集中处理。

### 将一个类声明为 Bean 的注解有哪些？

- `@Component`：通用组件注解，适合无法明确归类到具体层的 Bean。
- `@Repository`：持久层组件，通常用于 DAO、Mapper 适配类或数据库访问对象。
- `@Service`：业务服务层组件，通常承载领域逻辑或应用服务逻辑。
- `@Controller`：Spring MVC 控制层组件，负责接收请求并返回视图或响应数据。
- `@RestController`：组合了 `@Controller` 和 `@ResponseBody`，常用于 REST API。

### `@Component` 和 `@Bean` 的区别是什么？

- `@Component` 作用在类上，通常配合组件扫描使用，由 Spring 自动发现并注册 Bean。
- `@Bean` 作用在方法上，通常写在 `@Configuration` 配置类中，由方法返回值注册 Bean。
- 引入第三方类时，如果无法修改源码添加 `@Component`，通常使用 `@Bean` 显式声明。

### 注入 Bean 的注解有哪些？

常见注入注解包括 `@Autowired`、`@Resource`、`@Inject`。实际项目中，`@Autowired` 和 `@Resource` 更常见。

### `@Autowired` 和 `@Resource` 的区别是什么？

`@Autowired` 是 Spring 提供的注解，默认按类型匹配；如果同类型 Bean 有多个，再结合名称或 `@Qualifier` 指定具体 Bean。

`@Resource` 来自 JSR-250 规范，默认按名称匹配；找不到同名 Bean 时，Spring 会回退到按类型匹配。需要明确指定时，可以使用 `@Resource(name = "xxx")`。

面试回答时可以抓住两点：来源不同、默认匹配方式不同。

### 注入 Bean 的方式有哪些？

依赖注入常见方式包括：

1. 构造器注入：通过构造函数传入依赖。
2. Setter 注入：通过 Setter 方法设置依赖。
3. 字段注入：直接在字段上使用 `@Autowired`、`@Resource` 等注解。

### 构造函数注入还是 Setter 注入？

Spring 官方文档建议：构造器注入更适合必需依赖，Setter 注入更适合可选依赖。

构造器注入的优势是依赖完整性更强，字段可以声明为 `final`，对象创建后依赖关系更清晰，也更方便单元测试。Setter 注入适合有默认值、可选项、运行期可调整的依赖。

### Bean 的作用域有哪些？

常见 Bean 作用域包括：

- `singleton`：IoC 容器中只有一个 Bean 实例，Spring 默认作用域。
- `prototype`：每次获取 Bean 都创建一个新实例。
- `request`：每次 HTTP 请求创建一个 Bean，仅 Web 应用可用。
- `session`：每个 HTTP Session 创建一个 Bean，仅 Web 应用可用。
- `application`：每个 ServletContext 创建一个 Bean，仅 Web 应用可用。
- `websocket`：每个 WebSocket 会话创建一个 Bean，仅 Web 应用可用。

### Bean 是线程安全的吗？

Bean 是否线程安全取决于作用域和状态。

`prototype` 每次获取都是新对象，通常不存在共享状态竞争。`singleton` 是默认作用域，同一个 Bean 会被多个线程共享；如果 Bean 没有可变成员变量，通常是线程安全的；如果保存了可变状态，就需要避免共享、使用 `ThreadLocal`，或者通过同步机制处理并发访问。

### Bean 的生命周期了解吗？

Bean 生命周期可以先按四步记忆：

1. 实例化：根据 BeanDefinition 创建对象。
2. 属性注入：给对象注入依赖和配置属性。
3. 初始化：执行初始化逻辑，此时对象已经可以被 Spring 容器正常管理。
4. 销毁：容器关闭时执行销毁逻辑，释放连接、线程池等资源。

面试回答到这四步通常就够了。更细的扩展点可以知道它们存在，但不需要作为背诵重点。

## Spring AOP

### 谈谈自己对于 AOP 的了解

AOP（Aspect-Oriented Programming，面向切面编程）用于处理横跨多个业务模块的逻辑，例如日志、事务、权限、监控、缓存等。

Spring AOP 默认基于代理实现：如果目标对象实现了接口，通常使用 JDK 动态代理；如果目标对象没有实现接口，则使用 CGLIB 创建目标类的子类代理。

![Spring AOP 代理机制](/img/技术/框架/spring-aop-proxy.png)

### AOP 常见术语

- **Target**：被代理的目标对象。
- **Proxy**：对目标对象应用增强逻辑后生成的代理对象。
- **JoinPoint**：可以被增强的位置，在 Spring AOP 中通常是方法执行点。
- **Pointcut**：实际被匹配和拦截的连接点。
- **Advice**：增强逻辑，例如前置通知、后置通知、异常通知、环绕通知。
- **Aspect**：切面，通常由 Pointcut 和 Advice 组成。

### Spring AOP 和 AspectJ AOP 有什么区别？

Spring AOP 是运行时增强，基于动态代理，主要支持 Spring Bean 的方法级增强，使用简单，适合大多数 Spring 应用中的事务、日志、权限等场景。

AspectJ 能在编译期或类加载期织入，能力更完整，可以增强字段、构造器、静态方法等更多连接点，但引入和理解成本更高。

面试回答时可以总结为：简单 Spring 业务场景优先 Spring AOP；需要更完整织入能力或更复杂切点时考虑 AspectJ。

### AOP 常见的通知类型有哪些？

- `Before`：目标方法执行前触发。
- `After`：目标方法执行后触发，不关心成功还是异常。
- `AfterReturning`：目标方法正常返回后触发。
- `AfterThrowing`：目标方法抛出异常后触发。
- `Around`：环绕通知，可以在目标方法执行前后编写逻辑，也可以决定是否继续调用目标方法。

## Spring MVC

Spring MVC 是 Spring Framework 中基于 Servlet API 的 Web MVC 框架。它的核心入口是 `DispatcherServlet`，请求进入后由它统一分发给对应的处理器。

### Spring MVC 的核心组件有哪些？

- `DispatcherServlet`：前端控制器，负责接收请求、协调处理流程并返回响应。
- `HandlerMapping`：根据请求路径、请求方法等信息找到对应的处理器。
- `HandlerAdapter`：适配并调用具体的处理器方法。
- `Handler`：实际处理请求的处理器，通常对应 Controller 方法。
- `ViewResolver`：在传统服务端渲染场景下解析视图。

### Spring MVC 工作原理了解吗？

典型流程如下：

1. 客户端请求进入 `DispatcherServlet`。
2. `DispatcherServlet` 调用 `HandlerMapping` 查找可以处理请求的 `Handler`。
3. `DispatcherServlet` 调用 `HandlerAdapter` 执行对应的 `Handler`。
4. `Handler` 执行业务逻辑后返回结果。
5. 传统 MVC 场景中返回 `ModelAndView`，再由 `ViewResolver` 解析并渲染视图。
6. 前后端分离场景中，Controller 通常直接返回 JSON 数据，常用 `@RestController` 或 `@ResponseBody` 实现。

### 统一异常处理怎么做？

Spring MVC 常用 `@ControllerAdvice` 和 `@ExceptionHandler` 做统一异常处理。

`@ControllerAdvice` 用于声明全局 Controller 增强类，`@ExceptionHandler` 用于声明具体异常的处理方法。当 Controller 抛出异常时，Spring MVC 会选择匹配的异常处理方法生成响应。

## Spring 事务

### Spring 管理事务的方式有几种？

Spring 事务主要由 `spring-tx` 模块提供支持，常见方式有两种：

- **编程式事务**：通过 `TransactionTemplate` 或 `PlatformTransactionManager` 在代码中手动控制事务边界。
- **声明式事务**：通过 XML 或 `@Transactional` 声明事务规则，实际执行通常基于 AOP 代理机制。

### Spring 事务中有哪些事务传播行为？

事务传播行为用于定义一个事务方法被另一个事务方法调用时，事务应该如何传播。

- `REQUIRED`：当前存在事务则加入；不存在事务则新建事务。`@Transactional` 默认使用该传播行为。
- `REQUIRES_NEW`：始终新建事务；如果当前存在事务，则挂起当前事务。
- `NESTED`：当前存在事务时创建嵌套事务；不存在事务时行为类似 `REQUIRED`。
- `MANDATORY`：必须在已有事务中运行；如果当前没有事务则抛出异常。
- `SUPPORTS`：当前存在事务则加入；没有事务则以非事务方式运行。
- `NOT_SUPPORTED`：以非事务方式运行；如果当前存在事务，则挂起当前事务。
- `NEVER`：以非事务方式运行；如果当前存在事务则抛出异常。

### Spring 事务中的隔离级别有哪几种？

Spring 的事务隔离级别对应数据库隔离级别：

- `DEFAULT`：使用数据库默认隔离级别。
- `READ_UNCOMMITTED`：允许读取未提交数据。
- `READ_COMMITTED`：只能读取已提交数据。
- `REPEATABLE_READ`：同一事务内多次读取同一数据结果保持一致。
- `SERIALIZABLE`：最高隔离级别，事务串行执行。

### `@Transactional` 什么情况下会失效？

常见失效场景包括：

- 方法不是 `public`。
- 同一个类内部自调用，绕过了 Spring AOP 代理。
- 异常被捕获后没有继续抛出，事务拦截器感知不到异常。
- 默认只对 `RuntimeException` 和 `Error` 回滚，受检异常需要通过 `rollbackFor` 指定。
- 方法所在类没有被 Spring 容器管理。

## Spring Data JPA

Spring Data JPA 是 Spring Data 项目的一部分，用于简化基于 JPA 的数据访问层开发。它不是 JPA 规范本身，而是在 JPA 之上提供 Repository 抽象、方法名查询、分页排序、审计等能力。

### 如何使用 JPA 在数据库中非持久化一个字段？

如果某个字段不希望映射到数据库列，常见方式包括：

- 使用 `transient` 关键字。
- 使用 JPA 的 `@Transient` 注解。
- `static`、`final` 字段通常也不会作为普通持久化字段处理。

实际项目中，语义最明确的是使用 `@Transient`。

### JPA 的审计功能是做什么的？

审计功能用于记录数据的创建人、创建时间、最后修改人、最后修改时间等信息。

常见注解包括：

- `@CreatedDate`：创建时间。
- `@LastModifiedDate`：最后修改时间。
- `@CreatedBy`：创建人。
- `@LastModifiedBy`：最后修改人。

### 实体之间的关联关系注解有哪些？

- `@OneToOne`：一对一。
- `@OneToMany`：一对多。
- `@ManyToOne`：多对一。
- `@ManyToMany`：多对多。

## Spring Security

Spring Security 是 Spring 体系中的安全框架，主要处理认证、授权、密码存储、防护类安全能力，以及与 Web 安全相关的过滤器链。

### 有哪些控制请求访问权限的方法？

常见权限控制方法包括：

- `permitAll()`：允许所有请求访问。
- `anonymous()`：允许匿名用户访问。
- `denyAll()`：拒绝所有访问。
- `authenticated()`：只允许已认证用户访问。
- `hasRole()`、`hasAnyRole()`：按角色判断。
- `hasAuthority()`、`hasAnyAuthority()`：按权限判断。
- `hasIpAddress()`：按 IP 地址判断。

### `hasRole` 和 `hasAuthority` 有区别吗？

`hasAuthority("admin")` 会直接检查用户是否具有 `admin` 权限。

`hasRole("admin")` 通常会在内部加上 `ROLE_` 前缀，即检查 `ROLE_admin`。因此角色和权限的命名要保持一致，否则容易出现明明配置了权限却无法访问的问题。

### 如何对密码进行加密？

密码不应该明文存储。Spring Security 使用 `PasswordEncoder` 处理密码编码和匹配，核心方法包括：

- `encode()`：对原始密码进行编码。
- `matches()`：校验原始密码和已编码密码是否匹配。
- `upgradeEncoding()`：判断已编码密码是否需要升级编码方式。

常见做法是使用 BCrypt 相关实现，例如 `BCryptPasswordEncoder`。如果系统需要兼容多种密码算法，可以使用 `DelegatingPasswordEncoder`。

## 参考资料

- [Spring Framework Reference Documentation](https://docs.spring.io/spring-framework/reference/)
- [Spring Framework Overview](https://docs.spring.io/spring-framework/reference/overview.html)
- [Spring Web MVC Documentation](https://docs.spring.io/spring-framework/reference/web/webmvc.html)
- [Spring Data JPA Reference Documentation](https://docs.spring.io/spring-data/jpa/reference/)
- [Spring Security Reference Documentation](https://docs.spring.io/spring-security/reference/)
- [JavaGuide：Spring 常见面试题总结](https://javaguide.cn/system-design/framework/spring/spring-knowledge-and-questions-summary.html)

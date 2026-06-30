# 第2章 Spring Boot

## Spring Boot 解决了什么问题？

Spring Framework 已经通过 IoC、AOP、事务、Web MVC 等能力简化了企业级 Java 开发，但传统 Spring 项目仍然需要大量显式配置。例如 XML 或 Java Config、Servlet 和过滤器配置、第三方库集成配置、依赖版本管理等。

Spring Boot 建立在 Spring Framework 之上，目标是简化 Spring 应用的创建、配置、运行和运维。它通过自动配置、Starter 依赖、嵌入式服务器和生产级特性，让开发者可以更快搭建一个可运行的 Spring 应用。

一句话总结：**Spring Boot 不是替代 Spring，而是降低 Spring 应用的工程配置成本。**

## 为什么要有 Spring Boot？

Spring 的目标是简化 J2EE 企业应用开发；Spring Boot 的目标是进一步简化 Spring 应用开发。

传统 Spring 项目常见痛点包括：

1. 配置文件多，XML 或 Java Config 编写成本高。
2. 第三方库集成时需要手动声明很多 Bean。
3. Maven 或 Gradle 依赖版本容易冲突。
4. Web 项目通常需要外部 Servlet 容器。
5. 应用运行状态、健康检查、指标等生产级能力需要额外接入。

Spring Boot 通过约定优先的默认配置和开箱即用的集成能力解决这些问题。

## 使用 Spring Boot 的主要优点有哪些？

常见优点包括：

1. 快速创建基于 Spring 的应用程序。
2. 减少样板代码、XML 配置和重复工程配置。
3. 通过 Starter 简化依赖管理，降低版本冲突概率。
4. 更容易集成 Spring JDBC、Spring ORM、Spring Data、Spring Security 等 Spring 生态能力。
5. 默认提供固执己见的配置，但允许按需覆盖。
6. 支持嵌入式 Tomcat、Jetty、Undertow，普通 `java -jar` 就能启动 Web 应用。
7. 提供 Actuator 等生产级能力，用于健康检查、指标、环境信息等。
8. 与 Maven、Gradle 插件集成，便于打包、运行和测试。

## Spring Boot Starters 是什么？

Spring Boot Starters 是一组按场景组织好的依赖集合。它把某类开发场景需要的常见依赖组合在一起，开发者只需要引入一个 Starter，就能得到一组经过版本协调的依赖。

例如开发 REST API 或 Web 应用时，不需要手动逐个引入 Spring MVC、Jackson、Tomcat 等依赖，只需要引入：

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

`spring-boot-starter-web` 会带上 Spring MVC、JSON 序列化、嵌入式 Web 容器等常用依赖。

## Spring Boot 支持哪些内嵌 Servlet 容器？

Spring Boot 常见内嵌 Servlet 容器包括：

- **Tomcat**：默认 Web 容器，最常见。
- **Jetty**：轻量级 Web 容器，也常用于嵌入式场景。
- **Undertow**：高性能 Web 容器，来自 JBoss/WildFly 生态。

Spring Boot Web 应用可以打成可执行 Jar，通过内嵌容器直接启动；也可以按传统方式打成 War，部署到兼容的外部 Servlet 容器。

## 如何把默认 Tomcat 替换成 Jetty？

`spring-boot-starter-web` 默认包含 Tomcat。如果要使用 Jetty，需要排除 Tomcat，再引入 Jetty Starter。

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <exclusions>
    <exclusion>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
    </exclusion>
  </exclusions>
</dependency>

<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-jetty</artifactId>
</dependency>
```

Gradle 中也可以排除 Tomcat 后引入 Jetty：

```groovy
implementation("org.springframework.boot:spring-boot-starter-web") {
  exclude group: "org.springframework.boot", module: "spring-boot-starter-tomcat"
}
implementation("org.springframework.boot:spring-boot-starter-jetty")
```

## `@SpringBootApplication` 注解做了什么？

`@SpringBootApplication` 是 Spring Boot 应用的核心启动注解，可以看作三个注解的组合：

- `@SpringBootConfiguration`：本质上包含 `@Configuration`，表示当前类是配置类，可以注册 Bean 或导入其他配置。
- `@EnableAutoConfiguration`：启用自动配置机制。
- `@ComponentScan`：从当前启动类所在包开始扫描 `@Component`、`@Service`、`@Controller`、`@Repository` 等组件。

所以启动类通常放在项目的根包下，避免组件扫描范围不完整。

## Spring Boot 自动配置是如何实现的？

自动配置的入口是 `@EnableAutoConfiguration`。它通过 `@Import` 导入自动配置选择器，由选择器加载候选自动配置类，再交给 Spring 容器处理。

可以按下面流程理解：

1. 启动类标注 `@SpringBootApplication`。
2. `@SpringBootApplication` 间接启用 `@EnableAutoConfiguration`。
3. 自动配置选择器读取 Spring Boot 提供的自动配置类清单。
4. Spring 根据 classpath、已有 Bean、配置属性和 Web 应用类型等条件判断哪些配置生效。
5. 满足条件的自动配置类注册对应 Bean。

自动配置能做到“按需生效”，核心依赖条件注解，例如：

- `@ConditionalOnClass`：classpath 中存在指定类时生效。
- `@ConditionalOnMissingBean`：容器中没有指定 Bean 时生效。
- `@ConditionalOnBean`：容器中存在指定 Bean 时生效。
- `@ConditionalOnProperty`：配置属性满足条件时生效。
- `@ConditionalOnWebApplication`：当前是 Web 应用时生效。

面试中回答自动配置时，重点说清楚三件事：**入口是 `@EnableAutoConfiguration`，加载的是自动配置类，是否生效由条件注解决定。**

## 开发 RESTful Web 服务常用哪些注解？

Spring Bean 相关注解：

- `@Component`：通用组件。
- `@Repository`：持久层组件。
- `@Service`：业务层组件。
- `@Controller`：MVC 控制器。
- `@RestController`：组合 `@Controller` 和 `@ResponseBody`，常用于 REST API。
- `@Autowired`：按类型自动注入 Bean。

HTTP 请求映射相关注解：

- `@GetMapping`：处理 GET 请求。
- `@PostMapping`：处理 POST 请求。
- `@PutMapping`：处理 PUT 请求。
- `@DeleteMapping`：处理 DELETE 请求。
- `@RequestMapping`：通用请求映射。

参数绑定相关注解：

- `@PathVariable`：获取路径参数。
- `@RequestParam`：获取查询参数或表单参数。
- `@RequestBody`：读取请求体，并通过 `HttpMessageConverter` 转成 Java 对象。

## Spring Boot 常用配置文件有哪些？

Spring Boot 常用配置文件是：

- `application.properties`
- `application.yml`
- `application.yaml`

不写配置时，Spring Boot 会使用默认配置；需要覆盖默认行为时，再通过配置文件、环境变量、命令行参数等方式指定。

## YAML 配置有什么优势？

YAML 是一种面向人类阅读的数据序列化格式，常用于配置文件。

相比 `properties`，YAML 更适合表达有层级的数据结构：

```yaml
server:
  port: 8080

spring:
  datasource:
    url: jdbc:mysql://localhost:3306/demo
    username: root
```

优势：

1. 层级结构清晰。
2. 适合复杂对象、列表、嵌套配置。
3. 可读性通常比大量点号分隔的 `properties` 更好。

需要注意：`@PropertySource` 默认更适合读取 `.properties` 文件，不适合直接导入自定义 YAML。

## 读取配置文件有哪些常用方式？

### 使用 `@Value`

适合读取少量简单配置：

```java
@Value("${app.name}")
private String appName;
```

缺点是配置分散、缺少类型聚合能力，不适合大量配置。

### 使用 `@ConfigurationProperties`

适合把一组同前缀配置绑定为一个配置对象：

```java
@Component
@ConfigurationProperties(prefix = "library")
public class LibraryProperties {
    private String location;
    private List<Book> books;

    // getter/setter
}
```

这种方式适合复杂配置，类型更清晰，也更方便测试。

### 配合校验使用

配置类可以配合 `@Validated` 和 Bean Validation 注解进行校验：

```java
@ConfigurationProperties(prefix = "my-profile")
@Validated
public class ProfileProperties {
    @NotEmpty
    private String name;

    @Email
    @NotEmpty
    private String email;
}
```

如果配置格式错误，应用启动时就会失败，避免错误配置进入运行期。

### 使用 `@PropertySource`

`@PropertySource` 常用于读取指定的 `.properties` 文件：

```java
@Component
@PropertySource("classpath:website.properties")
public class WebSite {
    @Value("${url}")
    private String url;
}
```

## Spring Boot 配置加载优先级了解吗？

Spring Boot 支持多种外部化配置来源，不同来源有优先级。常见优先级可以从“越靠近运行环境，优先级越高”来理解：

1. 命令行参数。
2. Java 系统属性。
3. 操作系统环境变量。
4. 应用外部的配置文件。
5. 应用内部 classpath 下的配置文件。
6. 默认属性。

实际排查配置不生效时，要重点检查是否被命令行参数、环境变量或外部配置文件覆盖。

## 常用 Bean 映射工具有哪些？

项目中经常需要在 DO、DTO、VO 等对象之间转换属性。常见 Bean 映射工具包括：

- Spring `BeanUtils`
- Apache `BeanUtils`
- MapStruct
- ModelMapper
- Dozer
- Orika
- JMapper

实际项目更推荐优先考虑 MapStruct。它通过编译期生成代码完成映射，性能和可维护性通常比反射型工具更好。

## Spring Boot 如何监控系统运行状态？

可以使用 Spring Boot Actuator。

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

引入后，应用可以暴露健康检查、指标、环境信息、应用信息等端点。常见端点包括：

- `/actuator/health`：健康检查。
- `/actuator/info`：应用信息。
- `/actuator/metrics`：指标信息。
- `/actuator/env`：环境和配置属性。

生产环境要注意端点暴露范围和访问权限，避免泄露敏感配置。

## Spring Boot 如何做请求参数校验？

Spring Boot Web 项目通常通过 Bean Validation 完成参数校验。常见注解包括：

- `@NotNull`：不能为 `null`。
- `@NotBlank`：字符串不能为 `null`，且去除空白后长度大于 0。
- `@NotEmpty`：集合或字符串不能为空。
- `@Size`：长度或集合大小在指定范围内。
- `@Min`、`@Max`：数值边界。
- `@Email`：邮箱格式。
- `@Pattern`：正则匹配。

请求体校验示例：

```java
@RestController
@RequestMapping("/api")
public class PersonController {

    @PostMapping("/person")
    public ResponseEntity<Person> create(@RequestBody @Valid Person person) {
        return ResponseEntity.ok(person);
    }
}
```

路径参数和查询参数校验，需要在类上添加 `@Validated`：

```java
@RestController
@RequestMapping("/api")
@Validated
public class PersonController {

    @GetMapping("/person/{id}")
    public ResponseEntity<Integer> getById(
            @PathVariable("id") @Max(value = 5, message = "超过 id 的范围了") Integer id) {
        return ResponseEntity.ok(id);
    }
}
```

## 如何实现全局异常处理？

Spring Boot Web 应用通常使用 `@ControllerAdvice` 和 `@ExceptionHandler` 做全局异常处理。

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<String> handleValidException(MethodArgumentNotValidException ex) {
        return ResponseEntity.badRequest().body("参数校验失败");
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleException(Exception ex) {
        return ResponseEntity.internalServerError().body("系统异常");
    }
}
```

`@RestControllerAdvice` 等价于 `@ControllerAdvice` 加 `@ResponseBody`，更适合 REST API。

## Spring Boot 如何实现定时任务？

使用 `@Scheduled` 声明定时任务，并在配置类或启动类上添加 `@EnableScheduling`。

```java
@SpringBootApplication
@EnableScheduling
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

```java
@Component
public class ScheduledTasks {

    @Scheduled(fixedRate = 5000)
    public void reportCurrentTime() {
        System.out.println("task running");
    }
}
```

常见调度参数：

- `fixedRate`：以上一次开始时间为基准，固定频率执行。
- `fixedDelay`：以上一次结束时间为基准，延迟指定时间后执行。
- `cron`：使用 Cron 表达式定义执行时间。

## Spring Boot 允许循环依赖发生吗？

Spring Boot 2.6 之后默认不允许循环依赖。确实需要兼容旧代码时，可以通过下面配置打开：

```properties
spring.main.allow-circular-references=true
```

不过这更像是兼容旧项目的手段。实际项目中出现循环依赖，通常说明职责边界或依赖方向需要重新设计，优先考虑重构。

## 面试回答重点

Spring Boot 面试中最常被追问的是自动配置和 Starter。回答时可以按下面思路组织：

1. Spring Boot 基于 Spring Framework，不是替代 Spring。
2. Starter 解决依赖组合和版本协调问题。
3. 自动配置根据 classpath、配置属性和已有 Bean 推断默认配置。
4. 条件注解决定自动配置是否生效。
5. Actuator、配置绑定、参数校验、全局异常和定时任务是项目中常见落地能力。

## 参考资料

- 《面试指北》：技术面试题篇 / 常见框架 / SpringBoot 常见面试题总结
- [Spring Boot Reference Documentation](https://docs.spring.io/spring-boot/reference/)
- [Spring Boot Auto-configuration](https://docs.spring.io/spring-boot/reference/using/auto-configuration.html)
- [Spring Boot Starters](https://docs.spring.io/spring-boot/reference/using/build-systems.html#using.build-systems.starters)

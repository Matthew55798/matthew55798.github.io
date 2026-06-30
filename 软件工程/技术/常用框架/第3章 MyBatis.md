# 第3章 MyBatis

## MyBatis 是什么？

MyBatis 是一个 Java 持久层框架，核心作用是把 Java 方法和 SQL 语句关联起来，帮我们完成参数设置、SQL 执行、结果集映射和资源管理。

它不像 Hibernate 那样强调完整的对象关系映射，而是让开发者自己编写 SQL。这样做的好处是 SQL 可控、性能更容易分析，适合复杂查询、报表、遗留数据库和需要精细 SQL 优化的业务。

可以这样理解：

- JDBC：SQL、参数设置、结果集遍历、异常处理、资源释放都要自己写。
- MyBatis：SQL 仍然自己控制，但 JDBC 样板代码由框架处理。
- Hibernate/JPA：更偏全自动 ORM，通过对象关系模型生成或执行 SQL。

所以 MyBatis 常被称为半自动 ORM 框架。

## MyBatis 的核心组件有哪些？

常见核心组件包括：

- `SqlSessionFactoryBuilder`：根据配置构建 `SqlSessionFactory`。
- `SqlSessionFactory`：用于创建 `SqlSession`，通常一个应用只需要一个。
- `SqlSession`：对外提供执行 SQL、获取 Mapper 代理、提交事务等能力。
- `Mapper` 接口：业务代码调用的数据访问接口。
- `MappedStatement`：XML 或注解中一条 SQL 语句解析后的内部表示。
- `BoundSql`：最终生成的 SQL 以及参数映射信息。
- `Executor`：SQL 执行器，负责调度 Statement、缓存和事务相关逻辑。
- `StatementHandler`：处理 JDBC `Statement` 的创建、参数设置和执行。
- `ParameterHandler`：负责把 Java 参数设置到 JDBC 占位符上。
- `ResultSetHandler`：负责把 JDBC `ResultSet` 映射成 Java 对象。
- `TypeHandler`：负责 Java 类型和 JDBC 类型之间的转换。

## MyBatis 的执行流程

一次典型 Mapper 方法调用可以按下面流程理解：

1. Spring 或 MyBatis 创建 Mapper 接口的代理对象。
2. 业务代码调用 Mapper 接口方法。
3. 代理对象根据接口全限定名和方法名定位到对应的 `MappedStatement`。
4. MyBatis 根据入参解析动态 SQL，生成 `BoundSql`。
5. `Executor` 创建或复用 `StatementHandler`。
6. `ParameterHandler` 设置 SQL 参数。
7. JDBC 执行 SQL。
8. `ResultSetHandler` 处理结果集，并通过 `TypeHandler` 做类型转换。
9. 返回 Java 对象、集合、Map 或影响行数。

面试回答时重点说清楚：**Mapper 接口没有实现类，运行时通过动态代理执行对应的 `MappedStatement`。**

## `#{}` 和 `${}` 的区别是什么？

`#{}` 是参数占位符。MyBatis 会把它处理成 JDBC `PreparedStatement` 的 `?`，再通过参数绑定设置值。

示例：

```sql
select * from user where id = #{id}
```

最终会类似：

```sql
select * from user where id = ?
```

这种方式可以复用预编译语句，并且可以防止常见 SQL 注入。

`${}` 是文本替换。MyBatis 会把参数内容原样拼进 SQL。

示例：

```sql
select * from user order by ${orderBy}
```

如果 `orderBy` 是 `create_time desc`，最终 SQL 就会直接拼成：

```sql
select * from user order by create_time desc
```

`${}` 适合无法使用占位符的位置，例如动态表名、动态列名、排序字段等。但它有 SQL 注入风险，必须做白名单校验，不能直接接收用户输入。

## XML 映射文件常见标签有哪些？

最常见的 SQL 标签：

- `select`
- `insert`
- `update`
- `delete`

常见映射和复用标签：

- `resultMap`：定义结果集列和 Java 对象属性之间的映射关系。
- `sql`：定义可复用 SQL 片段。
- `include`：引入 `sql` 片段。
- `cache`：开启当前 namespace 的二级缓存。
- `cache-ref`：引用其他 namespace 的缓存配置。
- `selectKey`：处理不支持自增主键或需要提前生成主键的场景。

常见动态 SQL 标签：

- `if`
- `choose`、`when`、`otherwise`
- `trim`
- `where`
- `set`
- `foreach`
- `bind`

## Mapper 接口的工作原理是什么？

Mapper 接口通常和 XML 映射文件对应：

- Mapper 接口全限定名对应 XML 的 `namespace`。
- Mapper 方法名对应 SQL 标签的 `id`。
- 方法参数对应 SQL 入参。

MyBatis 会为 Mapper 接口生成 JDK 动态代理。调用接口方法时，代理对象会根据 `namespace + id` 找到对应的 `MappedStatement`，再执行 SQL 并返回结果。

例如：

```java
public interface UserMapper {
    User selectById(Long id);
}
```

对应：

```xml
<mapper namespace="com.example.UserMapper">
  <select id="selectById" resultType="com.example.User">
    select id, name from user where id = #{id}
  </select>
</mapper>
```

## Mapper 方法能重载吗？

不建议重载。

MyBatis 的映射语句以 XML 中的 `id` 定位，同一个 XML namespace 下 `id` 不能重复。虽然在某些情况下可以通过一个 SQL 语句配合动态 SQL 兼容多个重载方法，但这会降低可读性，也容易让参数绑定变复杂。

工程实践中更推荐使用明确的方法名，例如：

```java
User selectById(Long id);

List<User> selectByCondition(UserQuery query);
```

## MyBatis 动态 SQL 是做什么的？

动态 SQL 用来根据入参条件动态拼接 SQL。它解决的是传统字符串拼接 SQL 容易漏空格、漏逗号、条件混乱和注入风险的问题。

常见场景：

- 查询条件可选。
- 批量 `in` 查询。
- 动态更新非空字段。
- 多分支条件查询。
- 复用 SQL 片段。

示例：

```xml
<select id="selectByCondition" resultType="User">
  select id, name, age
  from user
  <where>
    <if test="name != null and name != ''">
      and name like concat('%', #{name}, '%')
    </if>
    <if test="age != null">
      and age = #{age}
    </if>
  </where>
</select>
```

`where` 标签会在存在条件时自动补上 `where`，并处理开头多余的 `and` 或 `or`。

## 动态 SQL 的执行原理

MyBatis 解析 XML 时，会把动态 SQL 节点解析成一棵 SQL 节点树。执行时根据传入参数，通过 OGNL 表达式判断条件是否成立，再拼接生成最终 SQL。

例如 `if test="name != null"` 会根据参数对象里的 `name` 值判断是否拼接对应 SQL 片段。

最终执行前，动态 SQL 会被转换成 `BoundSql`，里面包含最终 SQL 文本和参数映射信息。

## MyBatis 如何封装 SQL 执行结果？

MyBatis 常见结果映射方式有两种：

1. 使用 `resultType`：适合列名和属性名能自动匹配的简单场景。
2. 使用 `resultMap`：适合列名和属性名不一致、复杂对象、一对一、一对多等场景。

示例：

```xml
<resultMap id="userResultMap" type="User">
  <id column="id" property="id"/>
  <result column="user_name" property="name"/>
</resultMap>
```

有了映射关系后，MyBatis 会通过反射创建目标对象，并把结果集中的列值设置到对象属性上。

## `resultType` 和 `resultMap` 有什么区别？

`resultType` 用于简单映射，通常依赖自动映射规则：

```xml
<select id="selectById" resultType="User">
  select id, name from user where id = #{id}
</select>
```

`resultMap` 用于复杂映射，能显式指定列和属性的关系，也支持关联对象和集合：

```xml
<resultMap id="orderResultMap" type="Order">
  <id column="order_id" property="id"/>
  <result column="order_no" property="orderNo"/>
  <association property="user" javaType="User">
    <id column="user_id" property="id"/>
    <result column="user_name" property="name"/>
  </association>
</resultMap>
```

复杂 SQL、字段命名不一致、关联查询时优先使用 `resultMap`。

## MyBatis 支持关联查询吗？

支持。

常见关联关系包括：

- `association`：一对一或多对一。
- `collection`：一对多。

实现方式主要有两种：

1. 嵌套结果：通过一次 `join` 查询拿到主对象和关联对象的数据，再用 `resultMap` 组装对象图。
2. 嵌套查询：先查主对象，再按需执行另一条 SQL 查询关联对象。

嵌套结果 SQL 次数少，但结果集可能有重复行，需要依赖 `id` 标记去重。嵌套查询结构清晰，也更容易配合延迟加载，但可能产生 N+1 查询问题。

## MyBatis 支持延迟加载吗？

支持，但主要针对 `association` 和 `collection`。

开启延迟加载后，MyBatis 不会在查询主对象时立刻查询关联对象，而是在访问关联属性时再执行对应 SQL。

典型配置：

```xml
<settings>
  <setting name="lazyLoadingEnabled" value="true"/>
</settings>
```

延迟加载的基本原理是为目标对象创建代理对象。当访问关联属性时，代理拦截调用，触发预先保存的关联查询 SQL，把结果设置到对象上。

使用时要注意 N+1 查询问题。列表页如果逐条访问关联属性，可能导致大量额外 SQL。

## MyBatis 如何分页？

常见分页方式：

1. `RowBounds`：逻辑分页，本质上可能先查询较多结果再在内存中截取，不适合大数据量。
2. 手写物理分页 SQL：例如 MySQL 使用 `limit offset, size`。
3. 分页插件：拦截 SQL 执行过程，根据数据库方言改写 SQL。

分页插件的常见原理是拦截 `StatementHandler` 或 `Executor`，在 SQL 执行前把原 SQL 改写为带分页的 SQL。

例如：

```sql
select * from user
```

改写为：

```sql
select * from user limit 0, 10
```

实际项目中更推荐统一使用成熟分页插件或在 SQL 层明确写物理分页。

## MyBatis 插件机制了解吗？

MyBatis 插件基于拦截器和动态代理实现，只能拦截四类核心接口：

- `Executor`
- `StatementHandler`
- `ParameterHandler`
- `ResultSetHandler`

自定义插件通常需要：

1. 实现 `Interceptor` 接口。
2. 通过 `@Intercepts` 和 `@Signature` 指定要拦截的接口、方法和参数。
3. 在 `intercept()` 中编写增强逻辑。
4. 在 MyBatis 配置中注册插件。

分页、SQL 日志、数据权限、字段加解密等能力都可以基于插件机制实现。

## MyBatis 有哪些 Executor？

MyBatis 内置三种常见执行器：

- `SimpleExecutor`：每次执行 SQL 都创建新的 `Statement`，执行后关闭。
- `ReuseExecutor`：按 SQL 复用 `Statement`，减少重复创建开销。
- `BatchExecutor`：批处理执行器，把多条更新语句加入批处理，最后统一提交。

执行器作用范围受 `SqlSession` 生命周期限制。

## 如何执行批处理？

可以使用 `ExecutorType.BATCH` 创建 `SqlSession`，或在 Spring 集成环境中配置批处理执行器。

批处理适合大量 `insert`、`update`、`delete`。它会把多次更新加入 JDBC batch，再统一执行。

注意事项：

- 批处理不适合 `select`。
- 需要关注事务提交时机。
- 大批量数据要分批 flush，避免内存占用过高。

## MyBatis 缓存机制

MyBatis 有一级缓存和二级缓存。

### 一级缓存

一级缓存是 `SqlSession` 级别缓存，默认开启。

同一个 `SqlSession` 内，执行相同 SQL 且参数相同时，可能直接从缓存返回。执行 `insert`、`update`、`delete`、`commit`、`rollback` 等操作会清空相关缓存。

一级缓存生命周期很短，通常只在一次请求或一次事务内有效。

### 二级缓存

二级缓存是 namespace 级别缓存，需要显式开启。

它可以跨 `SqlSession` 共享，但实际项目中要谨慎使用，因为缓存一致性、对象序列化、事务边界和多表关联更新都会增加复杂度。

对于高并发项目，更常见做法是使用 Redis 等专门缓存系统，并在业务层明确控制缓存一致性。

## XML 映射文件的 `id` 可以重复吗？

同一个 namespace 下，SQL 语句的 `id` 不能重复。

不同 XML 文件如果 namespace 不同，`id` 可以重复，因为 MyBatis 最终使用 `namespace + id` 定位一条 SQL。

如果没有 namespace 或 namespace 混乱，重复 `id` 会导致映射冲突。因此最佳实践是让 Mapper 接口全限定名和 XML namespace 保持一致。

## MyBatis 可以映射枚举吗？

可以。

MyBatis 可以通过 `TypeHandler` 处理枚举和数据库字段之间的转换。常见方式包括：

- 存枚举名称。
- 存枚举序号。
- 存业务自定义 code。

如果枚举有稳定业务编码，建议使用自定义 `TypeHandler` 显式处理 code 和枚举值之间的映射，避免枚举顺序变化导致数据含义错误。

## MyBatis 与 Spring Boot 如何集成？

Spring Boot 项目通常使用 `mybatis-spring-boot-starter` 集成 MyBatis。

常见步骤：

1. 引入 Starter。
2. 配置数据源。
3. 配置 Mapper XML 路径。
4. 使用 `@Mapper` 或 `@MapperScan` 注册 Mapper 接口。
5. 在 Service 层调用 Mapper，并交给 Spring 管理事务。

示例：

```java
@SpringBootApplication
@MapperScan("com.example.mapper")
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

## 为什么说 MyBatis 是半自动 ORM？

Hibernate/JPA 这类全自动 ORM 框架更强调对象模型和数据库表之间的映射，很多 SQL 可以由框架自动生成。

MyBatis 虽然也能完成对象映射，但 SQL 通常由开发者手写。也就是说：

- 对象和结果集的映射由框架处理。
- SQL 编写和优化主要由开发者负责。

这就是“半自动”的含义。

## 面试回答重点

MyBatis 面试高频点可以按下面顺序复习：

1. `#{}` 和 `${}` 的区别，尤其是 SQL 注入风险。
2. Mapper 动态代理和 `MappedStatement` 定位规则。
3. 动态 SQL 标签和 OGNL 表达式。
4. `resultType`、`resultMap`、`association`、`collection`。
5. 分页插件和 MyBatis 插件机制。
6. `SimpleExecutor`、`ReuseExecutor`、`BatchExecutor`。
7. 一级缓存、二级缓存的作用域和失效场景。
8. 与 Spring Boot 集成时的 Mapper 扫描和事务管理。

## 参考资料

- [JavaGuide：MyBatis 常见面试题总结](https://javaguide.cn/system-design/framework/mybatis/mybatis-interview.html)
- [MyBatis 官方文档：Introduction](https://mybatis.org/mybatis-3/)
- [MyBatis 官方文档：Mapper XML Files](https://mybatis.org/mybatis-3/sqlmap-xml.html)
- [MyBatis 官方文档：Dynamic SQL](https://mybatis.org/mybatis-3/dynamic-sql.html)

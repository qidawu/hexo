---
title: Java 数据持久化系列（四）Spring JDBC 使用总结
date: 2018-02-20 22:26:25
updated:
tags: [Java, JDBC, Spring]
typora-root-url: ..
---

# Spring JDBC 简介

Spring 的 JDBC 框架承担了资源管理和异常处理的工作，从而简化了底层 JDBC API 代码，让我们只需编写从数据库读写数据所需的代码。具体特性如下：

* Spring 为读取和写入数据库的几乎所有错误提供了丰富的异常，且不与特定的持久化框架相关联（如下图）。异常都继承自的父类 `DataAccessException`，是一个非受检异常，无需捕获，因为 Spring 认为触发异常的很多问题是不能在 `catch` 代码块中修复，因此不强制开发人员编写 `catch` 代码块。这把是否要捕获异常的权利留给了开发人员。

  ![data-access-exceptions](/img/spring/jdbc/data-access-exceptions.png)

* Spring 将数据访问过程中固定的和可变的部分明确划分为两个不同的类：*模板（template）* 和 *回调（callback）*。模板管理过程中固定的部分（如事务控制、资源管理、异常处理），而回调处理自定义的数据访问代码（如 SQL 语句、绑定参数、整理结果集）。针对不同的持久化平台，Spring 提供了多个可选的模板：

  ![data-access-templates](/img/spring/jdbc/data-access-templates.png)

# 依赖安装

要在 Spring 中使用 JDBC，需要依赖 `spring-jdbc`。如果使用 Spring Boot 的话，可以直接导入起步依赖 `spring-boot-starter-jdbc`：

```xml
<!-- Spring JDBC 起步依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>

<!-- MySQL JDBC 驱动程序 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

`mvn dependency:tree` 分析传递依赖如下：

```
[INFO] +- mysql:mysql-connector-java:jar:8.0.13:compile
[INFO] \- org.springframework.boot:spring-boot-starter-jdbc:jar:2.1.2.RELEASE:compile
[INFO]    +- org.springframework.boot:spring-boot-starter:jar:2.1.2.RELEASE:compile
[INFO]    |  +- org.springframework.boot:spring-boot:jar:2.1.2.RELEASE:compile
[INFO]    |  |  \- org.springframework:spring-context:jar:5.1.4.RELEASE:compile
[INFO]    |  |     +- org.springframework:spring-aop:jar:5.1.4.RELEASE:compile
[INFO]    |  |     \- org.springframework:spring-expression:jar:5.1.4.RELEASE:compile
[INFO]    |  +- org.springframework.boot:spring-boot-autoconfigure:jar:2.1.2.RELEASE:compile
[INFO]    |  +- org.springframework.boot:spring-boot-starter-logging:jar:2.1.2.RELEASE:compile
[INFO]    |  |  +- ch.qos.logback:logback-classic:jar:1.2.3:compile
[INFO]    |  |  |  \- ch.qos.logback:logback-core:jar:1.2.3:compile
[INFO]    |  |  +- org.apache.logging.log4j:log4j-to-slf4j:jar:2.11.1:compile
[INFO]    |  |  |  \- org.apache.logging.log4j:log4j-api:jar:2.11.1:compile
[INFO]    |  |  \- org.slf4j:jul-to-slf4j:jar:1.7.25:compile
[INFO]    |  +- javax.annotation:javax.annotation-api:jar:1.3.2:compile
[INFO]    |  +- org.springframework:spring-core:jar:5.1.4.RELEASE:compile
[INFO]    |  |  \- org.springframework:spring-jcl:jar:5.1.4.RELEASE:compile
[INFO]    |  \- org.yaml:snakeyaml:jar:1.23:runtime
[INFO]    +- com.zaxxer:HikariCP:jar:3.2.0:compile
[INFO]    |  \- org.slf4j:slf4j-api:jar:1.7.25:compile
[INFO]    \- org.springframework:spring-jdbc:jar:5.1.4.RELEASE:compile
[INFO]       +- org.springframework:spring-beans:jar:5.1.4.RELEASE:compile
[INFO]       \- org.springframework:spring-tx:jar:5.1.4.RELEASE:compile
```

可见，`spring-boot-starter-jdbc` 引入了如下传递依赖：

- `spring-boot-starter`
  - `spring-boot-autoconfigure` Spring Boot 自动配置类
- `spring-jdbc` Spring JDBC 核心库
- `HikariCP`，Spring Boot 2 的默认数据库连接池
- ......

# 配置解析

`spring-boot-autoconfigure` 依赖内含几个关键的配置类，提供了如下外部配置：

* `org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration`，用于自动配置嵌入式数据源 或 连接池数据源

  ```yaml
  spring:
    datasource:
      driver-class-name:
      url:
      username:
      password:
  ```

* `org.springframework.boot.autoconfigure.jdbc.JndiDataSourceAutoConfiguration`，用于自动配置 JNDI 数据源

  ```yaml
  spring:
    datasource:
      jndi-name:
  ```

* `org.springframework.boot.autoconfigure.jdbc.XADataSourceAutoConfiguration`，用于自动配置分布式事务的数据源

  ```yaml
  spring:
    datasource:
      xa:
        data-source-class-name:
        properties:
  ```

* `org.springframework.boot.autoconfigure.jdbc.JdbcTemplateAutoConfiguration`，用于导入配置类：`JdbcTemplateConfiguration`、`NamedParameterJdbcTemplateConfiguration`

  ```yaml
  spring:
    jdbc:
      template:
        fetch-size: 
        max-rows: 
        query-timeout: 
  ```

* `org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration` 用于自动配置 `DataSourceTransactionManager`

* `org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration`

  ```yaml
  spring:
    transaction:
      defaultTimeout:
      rollbackOnCommitFailure:
  ```

# 使用 JDBC Template

## 配置数据源

为了让 `JdbcTemplate` 正常工作，只需要为其设置 `DataSource` 数据源即可。Spring Boot 下直接使用外部配置：

```yaml
spring:
  datasource:
    type: com.zaxxer.hikari.HikariDataSource  #使用 HikariCP，Spring Boot 2 的默认数据库连接池
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/test?useUnicode=true;characterEncoding=utf-8
    username: 
    password: 
```

如果未使用 Spring Boot，Java Config 如下：

```java
@Bean
public DataSource dataSource() {
    HikariDataSource ds = new HikariDataSource();
    ds.setDriverClassName("com.mysql.cj.jdbc.Driver");
    ds.setJdbcUrl("jdbc:mysql://localhost:3306/test?useUnicode=true;characterEncoding=utf-8");
    ds.setUsername("");
    ds.setPassword("");
    return ds;
}
```

## API 介绍

![JdbcOperations](/img/spring/jdbc/JdbcOperations.png)

* `org.springframework.jdbc.core.JdbcOperations` 是 Spring 封装 JDBC 操作的核心接口，提供的方法如下，基于**索引参数**进行 SQL 参数绑定。实现类为 `org.springframework.jdbc.core.JdbcTemplate`：

  ```java
  <T> T execute(...)
  <T> List<T> query(String, RowMapper<T>, Object...)  // 多列查询
  Map<String, Object> queryForMap(String, Object...)  // 单行多列查询
  <T> T queryForObject(String, Class<T>, Object...)  // 单行单列查询
  <T> T queryForObject(String, RowMapper<T>, Object...)  // 单行多列查询
  <T> List<T> queryForList(String, Class<T>, Object...)  // 多行单列查询
  List<Map<String, Object>> queryForList(String, Object...)  // 多行多列查询
  SqlRowSet queryForRowSet(...)
  int update(...)  // 执行单个增删改
  int[] batchUpdate(...)  // 执行批量增删改
  Map<String, Object> call(...)  // 执行存储过程和函数
  ......
  ```

* `org.springframework.jdbc.core.namedparam.NamedParameterJdbcOperations` 接口支持将值以**命名参数**的形式绑定到 SQL，实现类为 `org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate`，其底层使用的仍然是 `JdbcOperations`，是一个二次封装的 API，推荐使用。

如果使用 Spring Boot 的话，可以直接导入起步依赖 `spring-boot-starter-jdbc`，会引入自动配置类 `org.springframework.boot.autoconfigure.jdbc.JdbcTemplateAutoConfiguration`，用于导入配置类：`JdbcTemplateConfiguration`、`NamedParameterJdbcTemplateConfiguration`，源码如下：

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnClass({ DataSource.class, JdbcTemplate.class })
@ConditionalOnSingleCandidate(DataSource.class)
@AutoConfigureAfter(DataSourceAutoConfiguration.class)
@EnableConfigurationProperties(JdbcProperties.class)
@Import({ JdbcTemplateConfiguration.class, NamedParameterJdbcTemplateConfiguration.class })
public class JdbcTemplateAutoConfiguration {

}
```

只要满足几个条件，该自动配置类就会生效：

* classpath 包含 `DataSource`、`JdbcTemplate` 类
* `DataSource` bean 有且只有一个

### JdbcOperations

依赖注入 `JdbcTemplate` 实现之后，使用如下：

```java
List<TestPO> testPOList = jdbcOperations.query(
        "SELECT id, name, city FROM test WHERE name = ? AND city = ?", 
        (rs, rowNum) -> new TestPO(
            rs.getLong("id"),
            rs.getString("name"),
            rs.getString("city")
        ), 
        "李四", "beijing"
    );
log.info("Result is {}", testPOList);  // Result is [TestPO(id=2, name=李四, city=beijing)]

TestPO testPO = jdbcOperations.queryForObject(
        "SELECT id, name, city FROM test WHERE id = ?", 
        (rs, rowNum) -> new TestPO(
            rs.getLong("id"),
            rs.getString("name"),
            rs.getString("city")
        ), 
        2
    );
log.info("Result is {}", testPO);  // Result is TestPO(id=2, name=李四, city=beijing)

String name = jdbcOperations.queryForObject("SELECT name FROM test WHERE id = ?", String.class, 2);
log.info("Result is {}", name);  // Result is 李四

List<String> names = jdbcOperations.queryForList("SELECT name FROM test WHERE city = ?", String.class, "beijing");
log.info("Result is {}", names);  // Result is [李四, 王五]

List<Map<String, Object>> testMapList = jdbcOperations.queryForList("SELECT id, name, city FROM test WHERE city = ?", "beijing");
log.info("Result is {}", testMapList);  // Result is [{id=2, name=李四, city=beijing}, {id=3, name=王五, city=beijing}]

Map<String, Object> testMap = jdbcOperations.queryForMap("SELECT id, name, city FROM test WHERE id = ?", 2);
log.info("Result is {}", testMap);  // Result is {id=2, name=李四, city=beijing}
```

### NamedParameterJdbcTemplate

使用 `JdbcOperations` 需要特别注意索引参数的正确顺序，如果在修改 SQL 时忘记修改参数顺序，将导致查询出错。因此更建议使用命名参数，按照名字来绑定值：

```java
Map<String, Object> cityParamMap = new HashMap<>();
paramMap.put("city", "beijing");
paramMap.put("name", "李四");

List<TestPO> testPOList = namedParameterJdbcOperations.query(
        "SELECT id, name, city FROM test WHERE name = :name AND city = :city", 
        paramMap, 
        (rs, rowNum) -> new TestPO(
            rs.getLong("id"),
            rs.getString("name"),
            rs.getString("city")
        )
    );

log.info("Result is {}", testPOList);  // Result is [TestPO(id=2, name=李四, city=beijing)]
```


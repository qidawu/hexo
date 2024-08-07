---
title: Java 学习资源总结
date: 2020-09-15 23:32:08
updated:
tags: [Java, JVM]
typora-root-url: ..
---

[Java Documentation](https://docs.oracle.com/en/java/index.html)

# Standard Edition

[Java Platform, Standard Edition Documentation](https://docs.oracle.com/en/java/javase/index.html)

## Roadmap

详见：[Oracle Java SE Support Roadmap](https://www.oracle.com/java/technologies/java-se-support-roadmap.html)

![Oracle Java SE Support Roadmap](/img/java/basics/oracle_javase_support_roadmap.png)

参考：

[一文详解｜从JDK8飞升到JDK17，再到未来的JDK21 | 阿里技术](https://mp.weixin.qq.com/s/IaUQrkxvaeEjujDe5-DVfA)

### Java SE 8

[Java Platform, Standard Edition (Java SE) Overview](https://docs.oracle.com/javase/8/docs/technotes/guides/)

Reference

- [Java SE API Documentation](https://docs.oracle.com/javase/8/docs/api/index.html)
- [JavaFX API Documentation](http://www.oracle.com/pls/topic/lookup?ctx=javase80&id=JFXAP)
- [Developer Guides](https://docs.oracle.com/javase/8/docs/index.html)
- [Java Language and Virtual Machine Specifications](https://docs.oracle.com/javase/specs/index.html)
- [Java SE Tools Reference for UNIX](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/index.html)
- [Java SE Tools Reference for Windows](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/index.html)



> Oracle has two products that implement Java Platform Standard Edition (Java SE) 8: 
>
> * Java SE Development Kit (JDK) 8. JDK 8 is a superset of JRE 8, and contains everything that is in JRE 8, plus tools such as the compilers and debuggers necessary for developing applets and applications. 
>
> * Java SE Runtime Environment (JRE) 8. JRE 8 provides the libraries, the Java Virtual Machine (JVM), and other components to run applets and applications written in the Java programming language. Note that the JRE includes components not required by the Java SE specification, including both standard and non-standard Java components.
>
> The following conceptual diagram illustrates the components of Oracle's Java SE products:

![Description of Java Conceptual Diagram](/img/java/basics/description_of_java_conceptual_diagram.png)

## JDK

一些术语：

> - [JCP](https://www.jcp.org/en/home/index) 是 Java Community Process（Java社区进程）的简称，社会各界组成的 Java 社区，规划和领导 Java 的发展。
> - [JSR](https://www.jcp.org/en/jsr/all) 是 Java Specification Requests（Java 规范请求）的简称，是  JCP 成员向委员会提交的 Java 发展议案，经过一系列流程后，如果通过会成为 JEP，最终会体现在未来的 Java 中。
> - [JEP](http://openjdk.java.net/jeps/0) 是 JDK Enhancement Proposals （Java 增强提案）的简称，为了保证日后 JDK 研发能够更加顺利地进行，从 JDK 8 开始，Oracle 启用 JEP 来定义和管理纳入新版 JDK 发布范围的功能特性。JDK 的版本变化将从这些提案中选取。例如：
>   - [Project Coin](http://openjdk.java.net/projects/coin)：目标是确定应向 JDK 7 中添加哪些小的语言特性。
>   - [Project Amber](http://openjdk.java.net/projects/amber)：目标是探索和孵化较小的、面向生产力的 Java 语言特性。
>   - [Project Loom](http://openjdk.java.net/projects/loom)：
>   - [Project Valhalla](http://openjdk.java.net/projects/valhalla)：
>   - [Project Panama](http://openjdk.java.net/projects/panama)：
>   - ......

一些历史：参考《[JDK 历代版本变化](https://github.com/JavaMilk/JDKHistory)》

> JDK 从 1.5 版本开始，在官方的正式文档与宣传资料中已经不再使用类似“JDK 1.5”的名称，只有程序员内部使用的开发版本号（Developer Version，例如 java -version 的输出）才继续沿用 1.5、1.6 和 1.7 的版本号（JDK 10 之后又改为了采用年份加月份作为开发版本号，例如 18.3），而公开版本号（Product Version）则改为 JDK 5、JDK 6 和 JDK 7 的命名方式。
>
> 从 JDK 10 开始，每年的 3 月和 9 月各发布一个大版本，目的就是避免众多功能特性被集中捆绑到一个 JDK 版本上而引发交付风险。同时为了降低维护成本，每六个 JDK 大版本中才会被划出一个长期支持（Long Term Suppot，LTS）版本，只有 LTS 版的 JDK 能够获得为期三年的支持和更新，普通版的 JDK 就只有短短六个月的生命周期。
>
> JDK 8 和 [JDK 11](https://www.oracle.com/java/technologies/javase/11all-relnotes.html) 是 LTS 版本，再下一个就到 2021 年发布的 JDK 17。

> 在 2018 年发布的 Java 11， Oracle 已经让 OpenJDK 和 Oracle JDK 两者的二进制文件在功能上尽可能相互接近，尽管 OpenJDK 与 Oracle JDK 两者在一些选项之间仍然存在一些差异。参考：<[Differences Between Oracle JDK and OpenJDK](https://www.baeldung.com/oracle-jdk-vs-openjdk)>。

![JDK](/img/java/basics/JDK.webp)

> 根据 [JEP-320](http://openjdk.java.net/jeps/320) 的内容，计划于 2018 年 9 月发布的 JDK 11 将不包括 Java EE 模块：JAX-WS（ JSR-224 ）、JAXB（ JSR-222 ）、JAF（ JSR-925 ）、Commons Annotations（ JSR-250 ）和 JTA（ JSR-907 ），而这些模块已在 JDK 中存在了多年。计划在 JDK 11 中移除的四个 Java EE 模块最终将进入 EE4J。



常见 JDK 版本：《[你该选择什么样的 JDK？](https://www.infoq.cn/article/HZHmj8LKWd652ZDXip1j)》

* Oracle JDK 商业版

  > OTN 协议下发行的传统的 Oracle JDK，个人可以免费使用，但若在生产环境中商用就必须付费，可以有三年时间的更新支持。

* [OpenJDK](http://openjdk.java.net/) 开源版

  > GPLv2+CE 协议下由 Oracle 发行的 OpenJDK，可以免费在开发、测试、生产环境中使用，但是只有半年时间的更新支持。
  >
  > 由于 Oracle 不愿意在旧版本的 OpenJDK 上继续耗费资源，而 RedHat (IBM) 又乐意扩大自己在 Java 社区的影响力，因此 RedHat 代替 Oracle 成为了 OpenJDK 历史版本的维护者，接过了 OpenJDK 6、7、8、11 的管理权力和维护职责。

* OpenJDK 的变种版本：

  * [AdoptOpenJDK builds](https://adoptopenjdk.net/)

  * AdoptOpenJDK OpenJ9 builds

    > 除了标准的 OpenJDK 构建外，AdoptOpenJDK 还提供了使用 OpenJ9 而非 HotSpot 的版本。OpenJ9 最初是由 IBM 实现的 JVM，现在已开源并交由 Eclipse Foundation 运作。
    >
    > 2017 年，IBM 将内部使用 20 多年之久的 J9 虚拟机开源，并贡献到 Eclipse Foundation。

  * [Red Hat build of OpenJDK](https://developers.redhat.com/products/openjdk/overview)

  * [Alibaba Dragonwell](https://www.aliyun.com/product/dragonwell)

  * [Azul Zulu](https://www.azul.com/)

  * [Tencent Kona 8](https://github.com/Tencent/TencentKona-8)、[Tencent Kona 11](https://github.com/Tencent/TencentKona-11)

  * ...

## JRE

> 不需要考虑 JDK 与 JRE 的关系了
>
> JDK 和 JRE 都是 Java 的重要组成部分，但它们的角色和用途是不同的。JDK 是 Java 开发工具包，主要用于开发 Java 应用。它包含了 JRE，同时还提供了一些额外的工具，如编译器（`javac`）、调试器（`jdb`）等。JRE 则是运行 Java 应用程序所需的环境。它包含了 Java 虚拟机（JVM）和 Java 类库，也就是 Java 应用程序运行时所需的核心类和其他支持文件。﻿
>
> 在 JDK 8 及之前的版本中，Oracle 会提供独立的 JRE 和 JDK 供用户下载。也就是说，你可以只安装 JRE 来运行 Java 程序，也可以安装 JDK 来开发 Java 程序。
>
> 然而从 JDK 9 开始，Oracle **不再单独发布 JRE**。取而代之的是 `jlink` 工具，可以使用这个工具来生成 **定制的运行时镜像**。这种方式简化了 Java 应用的部署，因你只需要分发包含你的应用和定制运行时镜像的包，不需要单独安装 JRE。

## JVM

除了官方 HotSpotVM、GraalVM 实现，其它厂商实现如下：

![](/img/java/jvm/jvm_impl.png)

参考：[Java 虚拟机系列](/tags/JVM/)

# Enterprise Edition

## Roadmap

作为分水岭，2017 年 11 月，Oracle 将 Java EE 移交给 Eclipse 基金会。 2018 年 3 月 5 日，Eclipse 基金会宣布 Java EE (Enterprise Edition) 被更名为 Jakarta EE。

参考：《[Java EE 规范重命名为 Jakarta EE](https://www.infoq.cn/article/62TVY0c7ANAm*rjdzKfj)》

- Jakarta EE 9 使用 jakarta 命名空间。《[Jakarta EE 9 Milestone 1 发布 从 javax.* 过渡到 jakarta.*](https://www.cnbeta.com/articles/soft/996081.htm)》
- Java EE 5（2005）到 Java EE 8（2017）使用 javax 命名空间。
- Java EE 4 使用 javax 命名空间。

| 规范                                                         | 创建组织                                                     | 发布时间     |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------ |
| Java EE 1.3 (JSR-058)                                        | Java Community Process                                       |              |
| Java EE 1.4 (JSR-151)                                        | Java Community Process                                       |              |
| [Java EE 5](https://docs.oracle.com/javaee/5/) (JSR-244)     | Java Community Process                                       | 2005         |
| [Java EE 6](https://docs.oracle.com/javaee/6/) (JSR-316)     | Java Community Process                                       | 2007         |
| [Java EE 7](https://docs.oracle.com/javaee/7/index.html) ([JSR-342](http://jcp.org/en/jsr/summary?id=342)) | Java Community Process                                       | May 28, 2013 |
| [Java EE 8](https://github.com/javaee/javaee-spec) ([JSR-366](http://jcp.org/en/jsr/summary?id=366)) | Java Community Process                                       | Aug 21, 2017 |
| [Jakarta EE 8](https://jakarta.ee/release/8/)                | Jakarta EE Platform Specification Project with guidance provided by the [Jakarta EE Working Group](https://jakarta.ee/) | 2019.9       |
| [Jakarta EE 9](https://jakarta.ee/release/9/)                | Jakarta EE Platform Specification Project with guidance provided by the [Jakarta EE Working Group](https://jakarta.ee/) | 2020.9       |

## Java EE

Java Platform, Enterprise Edition



Java EE 和 Spring 之间复杂的关系：

> Spring 诞生于 2004 年，由 Rod Johnson 发起，作为对 J2EE（Java 2 Platform，Enterprise Edition）和 EJB 2 复杂性的反击。从那个时候开始，Spring 和 Java EE 之间就没有停止过竞争，并彼此影响对方：
>
> * Spring（以及 Hibernate）的出现刺激了 Java EE 社区，促使他们推出了 EJB 3 和 JAP 1.0。
> * Spring Batch 直接影响到了 Batch 规范（JSR 352）。
> * Spring Dependency Injection 启发了 CDI（Context and Dependency Injection）。
> * Spring 恰到好处地使用了 J2EE 和 Java EE 中的某些标准，如 Servlet、JMS 和 JPA。
> * Spring 5 宣称兼容 Java EE 8。
>
> 从 2006 年开始，Java EE 也将提升易用性和对开发者的友好放在首位，但在演进速度方面还是很慢，主要有两个原因：
>
> * JCP 制定规范需要很长时间：即使是一个轻量级的规范，也需要多方参与，需要更长的时间才能达成一致。
> * 实现和认证：在规范发布之后，需要几个月时间才能找到符合认证的应用服务器。
>
> 而最近，这方面的差距在加大：
>
> * Spring Boot 将“以约定代替配置（Convention Over Configuration）”的原则发挥到了极致，进一步提升易用性。
> * Spring Cloud 利用 Netflix 的开源组件解决了与云原生应用开发相关的问题，如服务注册、服务发现、弹性、负载均衡、监控……
> * Spring 5 将响应式编程（Reactive Programming）提升为一等公民。
>
> Java EE 在这方面的速度要慢的多。在 2013 年发布 Java EE 7 之后，经历了一段消停期。2016 年，在社区的压力下，Oracle 才发布了一个新的路线图。
>
> Java EE 8 发布于 2017 年 9 月，虽然人们对其期望甚高，但并非革命性的。人们还是把更多的目光投向了 Java EE 9，期望下一个版本会有更多的创新。
>
> 与此同时，Eclipse 基金会于 2016 年中启动 Microprofile.io 项目，旨在以微服务架构为基准来优化企业版 Java，以此来推动 Java EE 生态系统的发展。Microprofile 1.0 涵盖了 JAX-RS 2.0、CDI 1.2 和 JSON-P 1.0，1.2 版本于 2017 年 9 月发布，加入了更多特性，如配置、容错、JWT、度量指标和健康检测，2.0 版本有望与 Java EE 8 看齐。

## Jakarta EE

[Eclipse Foundation Projects](https://projects.eclipse.org/list-of-projects)

* [Eclipse EE4J](https://projects.eclipse.org/projects/ee4j)
* https://jakarta.ee/specifications/
* https://eclipse-ee4j.github.io/jakartaee-tutorial/

### Architecture

Jakarta EE 平台的架构关系如下图所示。（请注意，此图显示了元素间的逻辑关系；它并不意味着将元素间物理关系为单独的机器、进程、地址空间或虚拟机。）

下面分别描述每个矩形及其之间的关系：

* 容器（如 Web Container）作为 Jakarta EE 运行时环境，为 Application Components（如 Server Pages、Servlet）提供所需服务；
* 而所提供的服务，由矩形下半部分的方框表示。例如， Web Container 为 Servlet 提供了 Bean Validation API。详见 [Jakarta EE 标准服务](https://jakarta.ee/specifications/platform/9/jakarta-platform-spec-9.html#a84)。
* Java SE 的 API 受 Java SE 运行时环境（JRE）的支持，适用于每种类型的 Application Components。
* 箭头表示需要访问 Jakarta EE 平台的其它部分。例如，Web Container 通过 JDBC™ API 为 Server Pages、Servlet 提供数据库访问能力。

![Jakarta EE Architecture Diagram](/img/java/basics/Jakarta_EE_Architecture_Diagram.svg)

### Application Components

Jakarta EE 运行时环境定义了 Jakarta EE 产品必须支持的四种 Application Components 类型：

| Application Components                                       | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Application clients                                          | 通常是在台式计算机上执行的 GUI 程序。提供类似于本机应用程序的用户体验，并且可以访问 Jakarta EE 中间层的所有设施。 |
| Applets                                                      | 通常是在 Web 浏览器中执行的 GUI 组件，但也可以在支持 Applet 编程模型的各种其它应用程序或设备中执行。 |
| Web Components ([Servlets](https://jakarta.ee/specifications/servlet/), [Server Pages](https://jakarta.ee/specifications/pages/), [Server Faces](https://jakarta.ee/specifications/faces/) Applications, Filters, and Web Event Listeners) | 通常在 Web 容器中执行，并可能响应来自 Web 客户端的 HTTP 请求。 |
| [Enterprise Beans](https://jakarta.ee/specifications/enterprise-beans/) | 在支持事务的托管环境中执行。可以使用 SOAP/HTTP 协议直接提供 Web 服务。 |

### Containers

容器为 Jakarta EE Application Components 提供运行时支持。

容器为 Application Components 提供了一套底层 Jakarta EE API 的联合视图。Jakarta EE Application Components 从不直接与其它  Jakarta EE  Application Components 交互。它们使用容器的协议和方法来相互交互以及与平台服务交互。在 Application Components 和  Jakarta EE 服务之间插入一个容器，可以使该容器透明地注入该组件所需的服务，例如声明式事务管理，安全检查，资源池和状态管理。

> This specification requires that containers provide a Java Compatible™ runtime environment, as defined by the Java Platform, Standard Edition, v8 specification (Java SE).

### Database

> The Jakarta EE platform requires a database, accessible through the JDBC API, for the storage of business data. The database is accessible from:
>
> * Web Components
> * Enterprise Beans
> * Application clients

### Jakarta EE Standard Services

https://jakarta.ee/specifications/platform/9/jakarta-platform-spec-9.html#a84

# 参考

教程：

- https://www.tutorialspoint.com/java8/index.htm

- https://www.tutorialspoint.com/java9/index.htm

- https://www.tutorialspoint.com/java/lang/java_lang_interfaces.htm

- https://www.tutorialspoint.com/java/util/index.htm

- https://www.tutorialspoint.com/java_generics/index.htm

- https://www.tutorialspoint.com/java_concurrency/index.htm

2017 年

- 《[Java 老矣，尚能饭否？——2017 Java 9 语言的发展现状以及未来](https://www.infoq.cn/article/is-java-out-of-date/)》

2019 年

- 《[InfoQ 2019 年 Java 发展趋势报告](https://www.infoq.cn/article/xHcGEQe2*jXl8kDTuNK2)》

  ![](/img/java/basics/java_2019Q3_graph.png)

- 《[2019 中国 Java 发展趋势报告](https://www.infoq.cn/article/WomHORGulYaYEDYwoiP8)》

  > Java 作为使用最为广泛的语言，最近几年还是有比较大进步的，无论从语法的易用性上还是性能上都有很大程度的提升。吸收了函数式编程的思想，lambda 表达式、Parallem stream、Var 变量等提升了开发人员的效率与代码的简洁性。ZGC 无疑是一项重大的改进，在一定程度上解决了 Java 天生的 GC 延迟问题。
  >
  > Java 的编程复杂度并没有明显的降低，比如 I/O 处理、并发 / 并⾏计算，以及类加载等等。再者是 Java 与操作系统之间的交互仍不够充分，尽管 Java 9 开始提供了不少的 API，然⽽了解和使用的群体不⾜。Java 在这方面明显不及 GO 语言。
  >
  > 从语⾔层⾯来看，Java 正在向主流非 Java 语⾔融合，解决其中鸿沟的关键是**语法的变化**，比如 Java 8 的 Lambda 表达式 和 Java 10 的局部变量类型（ `var` ）等。个人认为这是一件好事，未来前后端不分家，相互渗透，对于彼此语言都是良性。

2020 年

- 《[InfoQ Java 趋势发展报告（2020 年 9 月）](https://www.infoq.cn/article/aHx9jlWKrmJK5O6hKN4c)》
- 《[Java 现状与技术趋势报告 | 阿里技术](https://www.infoq.cn/article/HZHmj8LKWd652ZDXip1j)》
- 《[2020年 JVM 生态报告解读](https://segmentfault.com/a/1190000021906788)》
- 《[Java 的新未来：逐渐“Kotlin 化”](https://www.infoq.cn/article/YgPRotsrI1qSr8cxRGlq)》


2021 年

- 《[InfoQ 2021 年 Java 发展趋势报告](https://www.infoq.cn/article/mJxpWooi8uN1yN48dzBH)》

2022 年

- 《[InfoQ 2022 年趋势报告：Java 篇](https://www.infoq.cn/article/vl9UfPTm6oOPAUgYpN5c)》
- 《[2022 年 Java 将何去何从？](https://www.infoq.cn/article/Ckvyawmfma127oiA6Lt7)》

2023 年：

* 《[InfoQ 2023 年趋势报告：Java 篇](https://www.infoq.cn/article/PgTo5YAyrPszGXHiTbss)》



[从 Java 8 升级到 Java 17 踩坑全过程](https://mp.weixin.qq.com/s/d0U_d7D9RzEfn1u5STBNHg)

[从 JDK8 飞升到 JDK17，再到未来的 JDK21 | 阿里技术](https://mp.weixin.qq.com/s/YPJ-sfx04-fOGt4WuPDwIQ)

[升级指南之 JDK 11+ 新特性和阿里巴巴 AJDK | 阿里技术](https://mp.weixin.qq.com/s/4YtXx1JVsCdGoExpF8Se3Q)

[从 JDK 9 到 19，我们帮您提炼了和云原生场景有关的能力列表 | 阿里技术](https://xie.infoq.cn/article/1ca130cadfa18a513c6e04fde)

[从 JDK 9 到 19，认识一个新的 Java 形态（内存篇）| 阿里技术](https://mp.weixin.qq.com/s/cF6JgJIOCF6Jxg520rRbLA)

[从 JDK 9 开始，一些关键特性 | 阿里技术](https://mp.weixin.qq.com/s/Ul8-sidDZNtBuZJqhMxbGg)

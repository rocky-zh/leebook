# MyBatis 简介

---

MyBatis 是一个优秀的基于 Java 的持久层框架，它内部封装了 JDBC，使开发者只需关注SQL 语句本身，而不用再花费精力去处理诸如注册驱动、创建 Connection、配置 Statement等繁杂过程。

Mybatis 通过xml 或注解的方式将要执行的各种statement（statement、preparedStatement等）配置起来，并通过 Java 对象和 Statement 中 SQL 的动态参数进行映射生成最终执行的SQL 语句，最后由 MyBatis 框架执行 SQL 并将结果映射成 Java 对象并返回。


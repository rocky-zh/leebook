# Druid 简介

---

Druid 是阿里巴巴开源平台上的一个项目，整个项目由数据库连接池、插件框架和 SQL 解析器组成。该项目主要是为了扩展 JDBC 的一些限制，可以让程序员实现一些特殊的需求，比如向密钥服务请求凭证、统计 SQL 信息、SQL 性能收集、SQL 注入检查、SQL 翻译等，程序员可以通过定制来实现自己需要的功能。

## 各种连接池性能对比测试

测试执行申请归还连接 1,000,000（一百万）次总耗时性能对比。

### 环境

<table>
<tr><td>OS</td><td>OS X 10.8.2</td></tr>
<tr><td>CPU</td><td>intel i7 2GHz 4 core</td></tr>
<tr><td>JVM</td><td>java version "1.7.0_05"</td></tr>
</table>

### 基准测试结果

<table>
<tr><td>Jdbc Connection Pool</td><td>1 thread</td><td>2 threads</td><td>5 threads</td><td>10 threads</td><td>20 threads</td><td>50 threads</td></tr>
<tr><td>Druid</td><td>898</td><td>1,191</td><td>1,324</td><td>1,362</td><td>1,325</td><td>1,459</td></tr>
<tr><td>tomcat-jdbc</td><td>1,269</td><td>1,378</td><td>2,029</td><td>2,103</td><td>1,879</td><td>2,025</td></tr>
<tr><td>DBCP</td><td>2,324</td><td>5,055</td><td>5,446</td><td>5,471</td><td>5,524</td><td>5,415</td></tr>
<tr><td>BoneCP</td><td>3,738</td><td>3,150</td><td>3,194</td><td>5,681</td><td>11,018</td><td>23,125</td></tr>
<tr><td>jboss-datasource</td><td>4,377</td><td>2,988</td><td>3,680</td><td>3,980</td><td>32,708</td><td>37,742</td></tr>
<tr><td>C3P0</td><td>10,841</td><td>13,637</td><td>10,682</td><td>11,055</td><td>14,497</td><td>20,351</td></tr>
<tr><td>Proxool</td><td>16,337</td><td>16,187</td><td>18,310(Exception)</td><td>25,945</td><td>33,706(Exception)</td><td>39,501 (Exception)</td></tr>
</table>

### 结论

1. Druid 是性能最好的数据库连接池，tomcat-jdbc 和 druid 性能接近。
2. proxool 在激烈并发时会抛异常，完全不靠谱。
3. c3p0 和 proxool 都相当慢，慢到影响 sql 执行效率的地步。
4. bonecp 性能并不优越，采用 LinkedTransferQueue 并没有能够获得性能提升。
5. 除了 bonecp，其他的在 JDK 7 上跑得比 JDK 6 上快
6. jboss-datasource 虽然稳定，但是性能很糟糕
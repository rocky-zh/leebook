# 插入后直接获取 ID

---

## 修改映射文件

MySql 中在插入语句后执行如下语句，则会输出新插入记录的 id：

![](/assets/Lusifer1514310048.png)

或

![](/assets/Lusifer1514310132.png)

效果相同

![](/assets/Lusifer1514310169.png)

映射文件的 `<insert/>` 标签中，有一个子标签 `<selectKey/>` 用于获取新插入记录的主键值。以下两种写法均可以完成“使用新插入记录的主键值初始化被插入的对象”的功能。

```
<insert id="insertForId">
    INSERT INTO student(name, age, score) VALUES (#{name}, #{age}, #{score})
    <selectKey resultType="java.lang.Long" keyProperty="id" order="AFTER">
        SELECT @@IDENTITY
    </selectKey>
</insert>
```

* resultType：指出获取的主键的类型。
* keyProperty：指出主键在 Java 类中对应的属性名。此处会将获取的主键值直接封装到被插入的 Student 对象中。
* order：指出 id 的生成相对于 insert 语句的执行是在前还是在后。MySql 数据库表中的 id，均是先执行 insert 语句，而后生成 id，所以需要设置为 AFTER；Oracle 数据库表中的 id，则是在 insert 执行之前先生成，所以需要设置为 BEFORE。当前的 MyBatis 版本，不指定 order 属性，则会根据所用 DBMS，自动选择其值。

## 修改 DAO 实现类

```
public void insertForId(Student student) {
    try {
        sqlSession = MyBatisUtils.getSqlSession();
        sqlSession.insert("insertForId", student);
        sqlSession.commit();
    } finally {
        if (sqlSession != null) {
            sqlSession.close();
        }
    }
}
```

## 修改测试类

```
@Test
public void testInsertForId() {
    Student student = new Student();
    student.setName("赵六");
    student.setAge(31);
    student.setScore(82D);

    System.out.println("student = " + student);
    studentDao.insertForId(student);
    System.out.println("student = " + student);
}
```
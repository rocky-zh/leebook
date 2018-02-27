# sql 标签

---

`<sql/>` 标签用于定义 SQL 片断，以便其它 SQL 标签复用。而其它标签使用该 SQL 片断， 需要使用 `<include/>` 子标签。该 `<sql/>` 标签可以定义 SQL 语句中的任何部分，所以 `<include/>` 子标签可以放在动态 SQL 的任何位置。

## 修改映射文件

```
    <sql id="select">
        SELECT
            id,
            name,
            age,
            score
        FROM
          student
    </sql>
```

```
    <!-- foreach -->
    <select id="selectByForeachWithListCustom" resultType="com.lusifer.mybatis.entity.Student">
        <!-- select * from student where id in (2, 4) -->
        <include refid="select" />

        <if test="list != null and list.size > 0">
            WHERE id IN
            <foreach collection="list" open="(" close=")" item="student" separator=",">
                #{student.id}
            </foreach>
        </if>
    </select>
```

![](/assets/Lusifer1514414809.png)
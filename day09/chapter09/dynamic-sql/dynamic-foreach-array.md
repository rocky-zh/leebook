# foreach 标签-遍历数组

---

`<foreach/>` 标签用于实现对于数组与集合的遍历。对其使用，需要注意：

* collection 表示要遍历的集合类型，这里是数组，即 array。
* open、close、separator 为对遍历内容的 SQL 拼接。

本例实现的需求是，查询出 id 为 2 与 4 的学生信息。

## 定义 DAO 接口

```
    /**
     * 使用 foreach 标签查询
     * @param ids
     * @return
     */
    public List<Student> selectByForeach(Long[] ids);
```

## 定义映射文件

动态 SQL 的判断中使用的都是 OGNL 表达式。OGNL 表达式中的数组使用 `array` 表示，数组长度使用 `array.length` 表示。

![](/assets/Lusifer1514413085.png)

```
    <!-- foreach -->
    <select id="selectByForeach" resultType="com.lusifer.mybatis.entity.Student">
        <!-- select * from student where id in (2, 4) -->
        SELECT
            id,
            name,
            age,
            score
        FROM
          student
        <if test="array != null and array.length > 0">
            WHERE id IN
            <foreach collection="array" open="(" close=")" item="id" separator=",">
                #{id}
            </foreach>
        </if>
    </select>
```

## 修改测试类

```
    @Test
    public void selectByForeach() {
        Long[] ids = {2L, 4L};
        List<Student> studentList = dynamicStudentDao.selectByForeach(ids);
        for (Student student : studentList) {
            System.out.println(student);
        }
    }
```
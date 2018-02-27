# choose 标签

---

该标签中只可以包含 `<when/>` `<otherwise/>`，可以包含多个 `<when/>` 与一个 `<otherwise/>`。它们联合使用，完成 Java 中的开关语句 switch..case 功能。

本例要完成的需求是，若姓名不空，则按照姓名查询；若姓名为空，则按照年龄查询；若没有查询条件，则没有查询结果。

## 定义 DAO 接口

```
    /**
     * 使用 choose 标签查询
     * @param student
     * @return
     */
    public List<Student> selectByChoose(Student student);
```

## 定义映射文件

```
    <!-- choose -->
    <select id="selectByChoose" resultType="com.lusifer.mybatis.entity.Student">
        SELECT
            id,
            name,
            age,
            score
        FROM
          student
        <where>
            <choose>
                <when test="name != null and name != ''">
                    AND name LIKE concat('%', #{name}, '%')
                </when>
                <when test="age != null and age > 0">
                    AND age > #{age}
                </when>
                <otherwise>
                    AND 1 != 1
                </otherwise>
            </choose>
        </where>
    </select>
```

## 修改测试类

```
    @Test
    public void selectByChoose() {
        Student param = new Student();
//        param.setName("张三");
//        param.setAge(22);
        List<Student> studentList = dynamicStudentDao.selectByChoose(param);
        for (Student student : studentList) {
            System.out.println(student);
        }
    }
```
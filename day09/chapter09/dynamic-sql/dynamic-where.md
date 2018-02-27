# where 标签

---

`<if/>` 标签的中存在一个比较麻烦的地方：需要在 where 后手工添加 1=1 的子句。因为，若 where 后的所有<if/>条件均为 false，而 where 后若又没有 1=1 子句，则 SQL 中就会只剩下一个空的 where，SQL 出错。所以，在 where 后，需要添加永为真子句 1=1，以防止这种情况的发生。但当数据量很大时，会严重影响查询效率。

## 定义 DAO 接口

```
    /**
     * 使用 where 标签查询
     * @param student
     * @return
     */
    public List<Student> selectByWhere(Student student);
```

## 定义映射文件

```
    <!-- where-->
    <select id="selectByWhere" resultType="com.lusifer.mybatis.entity.Student">
        SELECT
            id,
            name,
            age,
            score
        FROM
          student
        <where>
            <if test="name != null and name != ''">
                AND name LIKE concat('%', #{name}, '%')
            </if>
            <if test="age != null and age > 0">
                AND age > #{age}
            </if>
        </where>
    </select>
```

## 修改测试类

```
    @Test
    public void selectByWhere() {
        Student param = new Student();
//        param.setName("张三");
//        param.setAge(22);
        List<Student> studentList = dynamicStudentDao.selectByWhere(param);
        for (Student student : studentList) {
            System.out.println(student);
        }
    }
```
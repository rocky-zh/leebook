# if 标签

---

对于该标签的执行，当 test 的值为 true 时，会将其包含的 SQL 片断拼接到其所在的 SQL
语句中。

本例实现的功能是：查询出满足用户提交查询条件的所有学生。用户提交的查询条件可以包含一个姓名的模糊查询，同时还可以包含一个年龄的下限。当然，用户在提交表单时可能两个条件均做出了设定，也可能两个条件均不做设定，也可以只做其中一项设定。

这引发的问题是，查询条件不确定，查询条件依赖于用户提交的内容。此时，就可使用动态 SQL 语句，根据用户提交内容对将要执行的 SQL 进行拼接。


## 定义 DAO 接口

```
    /**
     * 使用 if 标签查询
     * @param student
     * @return
     */
    public List<Student> selectByIf(Student student);
```

## 定义映射文件

为了解决两个条件均未做设定的情况，在 where 后添加了一个“1=1”的条件。这样就不至于两个条件均未设定而出现只剩下一个 where，而没有任何可拼接的条件的不完整 SQL 语句。

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.lusifer.mybatis.dao.DynamicStudentDao">
    <!-- if -->
    <select id="selectByIf" resultType="com.lusifer.mybatis.entity.Student">
        SELECT
            id,
            name,
            age,
            score
        FROM
            student
        WHERE 1 = 1
        <if test="name != null and name != ''">
            AND name LIKE concat('%', #{name}, '%')
        </if>
        <if test="age != null and age > 0">
            AND age > #{age}
        </if>
    </select>
</mapper>
```

## 修改测试类

```
    @Test
    public void selectByIf() {
        Student param = new Student();
//        param.setName("张三");
//        param.setAge(22);
        List<Student> studentList = dynamicStudentDao.selectByIf(param);
        for (Student student : studentList) {
            System.out.println(student);
        }
    }
```
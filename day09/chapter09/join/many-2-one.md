# 多对一关联查询

---

这里的多对一关联查询是指，在查询多方对象的时候，同时将其所关联的一方对象也查询出来。

由于在查询多方对象时也是一个一个查询，所以多对一关联查询，其实就是一对一关联查询。即**一对一关联查询的实现方式与多对一的实现方式是相同的**。

下面以省份 Province 与国家 Country 间的多对一关系进行演示。

## 定义实体类

```
package com.lusifer.mybatis.entity;

public class Province {
    private Long id;
    private String name;
    private Country country;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Country getCountry() {
        return country;
    }

    public void setCountry(Country country) {
        this.country = country;
    }
}
```

## 定义 DAO 接口

```
package com.lusifer.mybatis.dao;

import com.lusifer.mybatis.entity.Province;

public interface ProvinceDao {
    /**
     * 根据 ID 查询省份
     * @param id
     * @return
     */
    public Province getById(Long id);
}
```

## 定义测试类

```
package com.lusifer.mybatis.dao.test;

import com.lusifer.mybatis.dao.ProvinceDao;
import com.lusifer.mybatis.utils.MyBatisUtils;
import org.apache.ibatis.session.SqlSession;
import org.junit.After;
import org.junit.Before;

public class ProvinceDaoTest {
    private ProvinceDao provinceDao;
    private SqlSession sqlSession;

    @Before
    public void before() {
        sqlSession = MyBatisUtils.getSqlSession();
        provinceDao = sqlSession.getMapper(ProvinceDao.class);
    }

    @After
    public void after() {
        if (sqlSession != null) sqlSession.close();
    }
}
```

## 定义映射文件

### 多表连接查询方式

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.lusifer.mybatis.dao.ProvinceDao">
    <resultMap id="provinceMapper" type="com.lusifer.mybatis.entity.Province">
        <id column="id" property="id" />
        <result column="name" property="name" />
        <association property="country" javaType="com.lusifer.mybatis.entity.Country">
            <id column="cid" property="id" />
            <result column="cname" property="name" />
        </association>
    </resultMap>

    <select id="getById" resultMap="provinceMapper">
        SELECT
          a.id,
          a.name,
          b.id as cid,
          b.name AS cname
        FROM
          province AS a,
          country AS b
        WHERE a.id = #{id}
          and a.country_id = b.id
    </select>
</mapper>
```

注意，在映射文件中使用 `<association/>` 标签体现出两个实体对象间的关联关系。

* property：指定关联属性，即 Minister 类中的 country 属性
* javaType：关联属性的类型

### 修改测试类

```
    @Test
    public void getById() {
        Province province = provinceDao.getById(1L);
        System.out.println(province.getName());
        System.out.println(province.getCountry().getName());
    }
```

### 多表单独查询方式

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.lusifer.mybatis.dao.ProvinceDao">
    <select id="selectCountryById" resultType="com.lusifer.mybatis.entity.Country">
        SELECT id, name FROM country WHERE id = #{id}
    </select>

    <resultMap id="provinceMapper" type="com.lusifer.mybatis.entity.Province">
        <id column="id" property="id" />
        <result column="name" property="name" />
        <association
                property="country"
                javaType="com.lusifer.mybatis.entity.Country"
                select="selectCountryById"
                column="cid"
        />
    </resultMap>

    <select id="getById" resultMap="provinceMapper">
        SELECT
          a.id,
          a.name,
          b.id as cid,
          b.name AS cname
        FROM
          province AS a,
          country AS b
        WHERE a.id = #{id}
          and a.country_id = b.id
    </select>
</mapper>
```

![](/assets/Lusifer1514435492.png)
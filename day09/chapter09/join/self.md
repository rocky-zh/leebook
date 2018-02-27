# 自关联查询

---

所谓自关联是指，自己即充当一方，又充当多方，是 1:n 或 n:1 的变型。例如，对于新闻栏目 NewsColumn，可以充当一方，即父栏目，也可以充当多方，即子栏目。而反映到 DB 表中，只有一张表，这张表中具有一个外键，用于表示该栏目的父栏目。一级栏目没有父栏目，所以可以将其外键值设为 0，而子栏目则具有外键值。

为了便于理解，将自关联分为两种情况来讲解。一种是当作 1:n 讲解，即当前类作为一方，其包含多方的集合域属性。一种是当作 n:1 讲解，即当前类作为多方，其包含一方的域属性。

## 自关联的 DB 表

![](/assets/Lusifer1514437616.png)

## 以一对多的方式处理

以一对多方式处理，即一方可以看到多方。该处理方式的应用场景比较多，例如在页面上点击父栏目，显示出其子栏目。再如，将鼠标定位在窗口中的某菜单项上会显示其所有子菜单项等。

根据查询需求的不同，又可以分为两种情况：一种是查询出指定栏目的所有子孙栏目，一种是查询出指定栏目及其所有子孙栏目。

### 查询指定栏目的所有子孙栏目

根据指定的 id，仅查询出其所有子栏目。当然，包括其所有辈份的孙子栏目。即，给出的查询 id 实际为父栏目 id。

#### 定义实体类

```
package com.lusifer.mybatis.entity;

import java.util.Set;

public class NewsLabel {
    private Long id;
    private String name;
    // 关联属性，指定子栏目，即多方
    private Set<NewsLabel> subLab;

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

    public Set<NewsLabel> getSubLab() {
        return subLab;
    }

    public void setSubLab(Set<NewsLabel> subLab) {
        this.subLab = subLab;
    }
}
```

#### 定义 DAO 接口

```
package com.lusifer.mybatis.dao;

import com.lusifer.mybatis.entity.NewsLabel;

import java.util.List;

public interface NewsLabelDao {
    public List<NewsLabel> selectSubByParentId(Long parentId);
}
```

#### 定义映射文件

这里通过 select 语句的递归调用实现查询所有下级栏目的功能。查询结果的集合数据 `<collection/>` 来自于递归调用的 `selectSubByParentId` 查询。与第一次进行该查询不同的是， 第一次的 pid 动态参数值来自于调用方法传递来的实参，而 `<collection/>` 中查询语句的 pid 动态参数值来自于上一次的查询结果的 id 值。

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.lusifer.mybatis.dao.NewsLabelDao">
    <resultMap id="newsLabelMapper" type="com.lusifer.mybatis.entity.NewsLabel">
        <id column="id" property="id" />
        <result column="name" property="name" />
        <collection
                property="subLab"
                ofType="com.lusifer.mybatis.entity.NewsLabel"
                select="selectSubByParentId"
                column="id" />
    </resultMap>

    <select id="selectSubByParentId" resultMap="newsLabelMapper">
        SELECT id, name FROM news_label WHERE pid = #{pid}
    </select>
</mapper>
```

![](/assets/Lusifer1514438320.png)

#### 修改测试类

```
    @Test
    public void selectSubByParentId() {
        List<NewsLabel> newsLabels = newsLabelDao.selectSubByParentId(7L);
        for (NewsLabel newsLabel : newsLabels) {
            System.out.println(newsLabel.getName());
        }
    }
```

### 查询指定栏目及其所有子孙栏目

这里的查询结果，即要包含指定 id 的当前栏目，还包含其所有辈份的孙子栏目。即给出的 id 实际为当前要查询的栏目的 id。

#### 定义 DAO 接口

```
public NewsLabel getById(Long id);
```

#### 定义映射文件

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.lusifer.mybatis.dao.NewsLabelDao">
    <select id="selectSubByParentId" resultMap="newsLabelMapper">
        SELECT id, name FROM news_label WHERE pid = #{pid}
    </select>

    <resultMap id="newsLabelMapper" type="com.lusifer.mybatis.entity.NewsLabel">
        <id column="id" property="id" />
        <result column="name" property="name" />
        <collection
                property="subLab"
                ofType="com.lusifer.mybatis.entity.NewsLabel"
                select="selectSubByParentId"
                column="id" />
    </resultMap>

    <select id="getById" resultMap="newsLabelMapper">
        SELECT id, name FROM news_label WHERE id = #{id}
    </select>
</mapper>
```

#### 修改测试类

```
    @Test
    public void getById() {
        NewsLabel newsLabel = newsLabelDao.getById(1L);
        System.out.println(newsLabel.getName());
        Set<NewsLabel> subLab = newsLabel.getSubLab();
        for (NewsLabel label : subLab) {
            System.out.println(label.getName());
        }
    }
```

## 以多对一方式处理

以多对一方式处理，即多方可以看到一方。该处理方式的应用场景，例如在网页上显示当前页面的站内位置。

### 定义实体类

```
package com.lusifer.mybatis.entity;

public class NewsLabel {
    private Long id;
    private String name;
    private NewsLabel newsLabel;

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

    public NewsLabel getNewsLabel() {
        return newsLabel;
    }

    public void setNewsLabel(NewsLabel newsLabel) {
        this.newsLabel = newsLabel;
    }
}
```

### 定义 DAO 接口

```
public NewsLabel getParentByParentId(Long parentId);
```

### 定义映射文件

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.lusifer.mybatis.dao.NewsLabelDao">
    <resultMap id="newsLabelMapper" type="com.lusifer.mybatis.entity.NewsLabel">
        <id column="id" property="id" />
        <result column="name" property="name" />
        <collection
                property="subLab"
                ofType="com.lusifer.mybatis.entity.NewsLabel"
                select="getParentByParentId"
                column="pid" />
    </resultMap>

    <select id="getParentByParentId" resultMap="newsLabelMapper">
        SELECT id, name, pid FROM news_label WHERE id = #{id}
    </select>

</mapper>
```

### 修改测试类

```
    @Test
    public void getParentByParentId() {
        NewsLabel newsLabel = newsLabelDao.getParentByParentId(11L);
        System.out.println("子目录名称：" + newsLabel.getName());
        System.out.println("父目录名称：" + newsLabel.getNewsLabel().getName());
    }
```
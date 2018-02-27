# SpringMVC 表单标签库

---

在使用 SpringMVC 的时候我们可以使用 Spring 封装的一系列表单标签，这些标签都可以访问到 ModelMap 中的内容。

我们需要先在 JSP 中声明使用的标签，具体做法是在 JSP 文件的顶部加入以下指令：

```
<%@taglib uri="http://www.springframework.org/tags/form" prefix="form"%>
```
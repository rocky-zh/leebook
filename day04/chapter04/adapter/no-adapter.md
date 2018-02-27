# 不使用适配器

---

若不使用适配器模式，则调用者需要定义出所有的工种对象，然后逐个工种对象的工作方法进行调用。有 30 个工种，就应调用 30 个工作方法。很麻烦。

![](/assets/10001.png)

```
package com.lusifer.adapter.test;

import com.lusifer.adapter.service.ProgrammerService;
import com.lusifer.adapter.service.TeacherService;
import com.lusifer.adapter.service.impl.JavaProgrammer;
import com.lusifer.adapter.service.impl.MathTeacher;

public class MyTest {
    public static void main(String[] args) {
        TeacherService teacherService = new MathTeacher();
        ProgrammerService programmerService = new JavaProgrammer();

        System.out.println(teacherService.teach());
        System.out.println(programmerService.program());
    }
}
```
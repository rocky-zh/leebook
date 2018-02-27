# 基本元素定义

---

适配器的例子：教师接口 TeacherService，程序员接口 ProgrammerService，分别定义他们各自工种的具体工作。有分别定义了数学老师 MathTeacher 和 Java 程序员 JavaProgrammer。这些不同的工种所做的工作都各自是不同的，无法进行统一管理，协同工作。所以，此时就需要定义一个员工适配器接口 WorkerAdapter，用于将这些不同的工种进行统一管理。

## 定义教师接口

```
package com.lusifer.adapter.service;

/**
 * 定义教师接口
 * <p>Title: TeacherService</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/21 1:38
 */
public interface TeacherService {
    public String teach();
}
```

## 定义程序员接口

```
package com.lusifer.adapter.service;

/**
 * 定义程序员接口
 * <p>Title: ProgrammerService</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/21 1:38
 */
public interface ProgrammerService {
    public String program();
}
```

## 定义数学老师具体实现

```
package com.lusifer.adapter.service.impl;

import com.lusifer.adapter.service.TeacherService;

/**
 * 定义数学老师的具体实现
 * <p>Title: MathTeacher</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/21 1:39
 */
public class MathTeacher implements TeacherService {
    public String teach() {
        return "数学老师教数学";
    }
}
```

## 定义 Java 程序员的具体实现

```
package com.lusifer.adapter.service.impl;

import com.lusifer.adapter.service.ProgrammerService;

/**
 * 定义 Java 程序员的具体实现
 * <p>Title: JavaProgrammer</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/21 1:40
 */
public class JavaProgrammer implements ProgrammerService {
    public String program() {
        return "Java 程序员编写 Java 代码";
    }
}
```
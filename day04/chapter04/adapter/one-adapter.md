# 使用一个适配器

---

![](/assets/10003.png)

## 定义工种适配器接口

```
package com.lusifer.adapter.service;

/**
 * 工种适配器
 * <p>Title: WorkerAdapter</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/21 1:52
 */
public interface WorkAdapter {
    public void work(Object worker);
}
```

## 定义工种适配器实现

```
package com.lusifer.adapter.service.impl;

import com.lusifer.adapter.service.ProgrammerService;
import com.lusifer.adapter.service.TeacherService;
import com.lusifer.adapter.service.WorkAdapter;

/**
 * 实现工种适配器接口
 * <p>Title: MyWorkAdapter</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/21 1:56
 */
public class MyWorkAdapter implements WorkAdapter {
    public void work(Object worker) {
        if (worker instanceof TeacherService) {
            System.out.println(((TeacherService) worker).teach());
        }

        else if (worker instanceof ProgrammerService) {
            System.out.println(((ProgrammerService) worker).program());
        }
    }
}
```

## 测试适配器

```
public static void main(String[] args) {
    TeacherService teacherService = new MathTeacher();
    ProgrammerService programmerService = new JavaProgrammer();
    Object[] workers = {teacherService, programmerService};

    WorkAdapter adapter = new MyWorkAdapter();
    for (Object worker : workers) {
        adapter.work(worker);
    }
}
```
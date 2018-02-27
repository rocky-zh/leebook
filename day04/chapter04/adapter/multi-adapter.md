# 分别配置适配器

---

![](/assets/10002.png)

## 定义新的适配器接口

```
package com.lusifer.adapter.service;

/**
 * 定义新的工种适配器
 * <p>Title: NewWorkAdapter</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/21 2:22
 */
public interface NewWorkAdapter {
    public void work(Object worker);

    /**
     * 判断是否支持该工种
     * @param worker
     * @return
     */
    public boolean supports(Object worker);
}
```

## 分别定义具体的适配器实现

```
package com.lusifer.adapter.service.impl;

import com.lusifer.adapter.service.NewWorkAdapter;
import com.lusifer.adapter.service.ProgrammerService;

/**
 * 实现程序员适配器
 * <p>Title: ProgrammerAdapter</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/21 2:28
 */
public class ProgrammerAdapter implements NewWorkAdapter {
    public void work(Object worker) {
        System.out.println(((ProgrammerService)worker).program());
    }

    public boolean supports(Object worker) {
        return worker instanceof ProgrammerService;
    }
}
```

```
package com.lusifer.adapter.service.impl;

import com.lusifer.adapter.service.NewWorkAdapter;
import com.lusifer.adapter.service.TeacherService;

/**
 * 实现教师适配器
 * <p>Title: MathTeacherAdapter</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/21 2:25
 */
public class TeacherAdapter implements NewWorkAdapter {
    public void work(Object worker) {
        System.out.println(((TeacherService)worker).teach());
    }

    public boolean supports(Object worker) {
        return worker instanceof TeacherService;
    }
}
```

## 测试适配器

```
public static void main(String[] args) {
    TeacherService teacherService = new MathTeacher();
    ProgrammerService programmerService = new JavaProgrammer();
    Object[] workers = {teacherService, programmerService};

    for (Object worker : workers) {
        NewWorkAdapter adapter = getAdapter(worker);
        adapter.work(worker);
    }
}

/**
 * 获取适配器
 * @param worker
 * @return
 */
private static NewWorkAdapter getAdapter(Object worker) {
    // 装载全部适配器
    List<NewWorkAdapter> adapters = new ArrayList<NewWorkAdapter>();
    adapters.add(new TeacherAdapter());
    adapters.add(new ProgrammerAdapter());
    for (NewWorkAdapter adapter : adapters) {
        // 判断是否支持
        if (adapter.supports(worker)) {
            return adapter;
        }
    }
    return null;
}
```
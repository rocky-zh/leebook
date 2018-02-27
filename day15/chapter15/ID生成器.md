# ID 生成器

---

系统唯一 ID 是我们在设计一个系统的时候常常会遇见的问题，也常常为这个问题而纠结。生成 ID 的方法有很多，适应不同的场景、需求以及性能要求。所以有些比较复杂的系统会有多个 ID 生成的策略。

## 简单的 ID 生成工具

```
package com.lusifer.myshop.common.utils;

import java.util.Random;

/**
 * ID 生成工具
 * <p>Title: IDUitls</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/2 11:37
 */
public class IDUtils {
    /**
     * id生成
     */
    public static long genId() {
        // 取当前时间的长整形值包含毫秒
        long millis = System.currentTimeMillis();
        // 加上两位随机数
        Random random = new Random();
        int end2 = random.nextInt(99);
        // 如果不足两位前面补0
        String str = millis + String.format("%02d", end2);
        long id = new Long(str);
        return id;
    }

    public static void main(String[] args) {
        System.out.println(genId());
    }
}
```
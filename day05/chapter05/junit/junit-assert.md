# JUnit 断言

---

| 断言                                                                                       | 描述                                                                                                              |
|--------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| void assertEquals([String message], expected value, actual value)                          | 断言两个值相等。值可能是类型有 int, short, long, byte, char or java.lang.Object. 第一个参数是一个可选的字符串消息 |
| void assertTrue([String message], boolean condition)                                       | 断言一个条件为真                                                                                                  |
| void assertFalse([String message],boolean condition)                                       | 断言一个条件为假                                                                                                  |
| void assertNotNull([String message], java.lang.Object object)                              | 断言一个对象不为空(null)                                                                                          |
| void assertNull([String message], java.lang.Object object)                                 | 断言一个对象为空(null)                                                                                            |
| void assertSame([String message], java.lang.Object expected, java.lang.Object actual)      | 断言，两个对象引用相同的对象                                                                                      |
| void assertNotSame([String message], java.lang.Object unexpected, java.lang.Object actual) | 断言，两个对象不是引用同一个对象                                                                                  |
| void assertArrayEquals([String message], expectedArray, resultArray)                       | 断言预期数组和结果数组相等。数组的类型可能是 int, long, short, char, byte or java.lang.Object.                    |

## 实例

```
package com.lusifer.use.junit.test;

import org.junit.Test;

import static org.junit.Assert.assertArrayEquals;
import static org.junit.Assert.assertEquals;
import static org.junit.Assert.assertNotNull;
import static org.junit.Assert.assertNotSame;
import static org.junit.Assert.assertNull;
import static org.junit.Assert.assertSame;
import static org.junit.Assert.assertTrue;

/**
 * 测试断言
 * <p>Title: AssertTest</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/22 2:13
 */
public class AssertTest {
    @Test
    public void test() {
        String obj1 = "junit";
        String obj2 = "junit";
        String obj3 = "test";
        String obj4 = "test";
        String obj5 = null;
        int var1 = 1;
        int var2 = 2;
        int[] arithmetic1 = {1, 2, 3};
        int[] arithmetic2 = {1, 2, 3};

        assertEquals(obj1, obj2);

        assertSame(obj3, obj4);

        assertNotSame(obj2, obj4);

        assertNotNull(obj1);

        assertNull(obj5);

        assertTrue("为真", var1 == var2);

        assertArrayEquals(arithmetic1, arithmetic2);
    }
}
```
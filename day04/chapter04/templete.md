# 模板方法模式

---

## 简介

在现实生活中，完成某件事情是需要 n 多个固定步骤的。如“在淘宝网进行购物”这件事情的完成一般需要三个步骤：登录网站、挑选商品、付款。但对于登录网站与付款这两步， 每个人几乎都是相同的操作。但不同的地方是，每个人所挑选的商品是不同的。

在软件开发过程中同样存在这样的情况。某类的某个方法的实现，需要几个固定步骤。在这些固定步骤中，对于该类的不同对象，有些步骤的实现是固定不变的，有些步骤的实现是大相径庭的，有些步骤的实现是可变可不变的。对于这种情况，就适合使用模板方法设计模式编程。

模板方法设计模式的定义是：定义一个操作中某种算法的框架，而将一些步骤延迟到子类中。模板方法模式使得子类在不改变一个算法结构的前提下，对某些步骤实现个性化定义。

## 模板方法程序结构

在模板方法设计模式中，存在一个父类。其中包含两类方法：模板方法与步骤方法。

* 模板方法，即实现某种算法的方法步骤。而这些步骤都是调用的步骤方法完成的。
* 步骤方法，即完成模板方法的每个阶段性方法。每个步骤方法完成某一特定的、完成总算法的一部分功能。步骤方法有三种类型：抽象方法、最终方法与钩子方法。
	* 抽象方法，是要求子类必须实现的方法，是完成模板方法的算法步骤中必须由子类完成的个性化定义。
	* 最终方法，是子类不能重写的方法，是若要完成模板方法的算法步骤，对于所有子类执行都一样的步骤。
	* 钩子方法，是父类给出了默认实现，但子类也可以重写的方法。

## 定义父类

```
package com.lusifer.templete.abstracts;

/**
 * 定义购物抽象类
 * <p>Title: ShoppingAbstract</p>
 * <p>Description: </p>
 *
 * @author Lusifer
 * @version 1.0.0
 * @date 2017/12/21 2:55
 */
public abstract class ShoppingAbstract {
    /**
     * 定义购物模板方法
     */
    public void shopping() {
        // 登录网站
        login();
        // 选择商品
        choice();
        // 支付
        pay();
    }

    /**
     * 钩子方法，可重写可不重写
     */
    protected void pay() {
        System.out.println("使用支付宝支付");
    }

    /**
     * 抽象方法，必须由子类重写
     */
    protected abstract void choice();

    /**
     * 最终方法，固定的
     */
    protected final void login() {
        System.out.println("登录网站");
    }
}
```

## 定义子类

```
package com.lusifer.templete.shopping;

import com.lusifer.templete.abstracts.ShoppingAbstract;

public class Book extends ShoppingAbstract {
    protected void choice() {
        System.out.println("购买了 Java 编程思想");
    }
}
```

```
package com.lusifer.templete.shopping;

import com.lusifer.templete.abstracts.ShoppingAbstract;

public class Food extends ShoppingAbstract {
    protected void choice() {
        System.out.println("购买了德芙巧克力");
    }

    @Override
    protected void pay() {
        System.out.println("使用微信支付");
    }
}
```

## 测试模板方法

```
package com.lusifer.templete.test;

import com.lusifer.templete.shopping.Book;
import com.lusifer.templete.shopping.Food;

public class MyTest {
    public static void main(String[] args) {
        Book book = new Book();
        book.shopping();
        System.out.println("-------------------------------");
        Food food = new Food();
        food.shopping();
    }
}
```
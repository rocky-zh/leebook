# 接口隔离原则

---

接口隔离原则（ISP：Interface Segregation Principle）

## 核心

不应该强迫客户程序依赖他们不需要使用的方法。接口隔离原则的意思就是：一个接口不需要提供太多的行为，一个接口应该只提供一种对外的功能，不应该把所有的操作都封装到一个接口当中.

## 接口隔离的两种实现方法

1. 使用委托隔离接口。（Separation through Delegation）
2. 使用多重继承隔离接口。（Separation through Multiple Inheritance）
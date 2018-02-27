# Java 中七种设计原则

---

类与类，类和接口，接口和接口之间的关系。 

1. 实现关系（一个类实现一个接口） 
2. 泛化关系（一个类继承另一个类） 
3. 关联
	* 依赖关系：一个类是另一个类的方法局部变量，方法的参数或方法返回值。
	* 聚合关系：一个类是另一个类的属性，是整体和部分的关系。
	* 组合关系：一个类是另一个类的属性，是整体不可分割的一部分，是强聚合。
4. 单一职责：一个类而言，应该仅有一个引起它变化的原因，永远不要让一个类存在多个改变的理由。一个类只应该做和一个任务相关的业务，不应该把过多的业务放在一个类中完成。 
5. 迪米特法则： 一个软件实体应当尽可能少的与其他实体发生相互作用。
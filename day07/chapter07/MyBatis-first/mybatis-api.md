# MyBatis 接口详解

---

Dao 中需要通过 SqlSession 对象来操作 DB。而 SqlSession 对象的创建，需要其工厂对象 SqlSessionFactory。SqlSessionFactory 对象，需要通过其构建器对象 SqlSessionFactoryBuilder 的 build() 方法，在加载了主配置文件的输入流对象后创建。

## Resources 类

Resources 类，顾名思义就是资源，用于读取资源文件。其有很多方法通过加载并解析资源文件，返回不同类型的 IO 流对象。

![](/assets/Lusifer1514226097.png)

## SqlSessionFactoryBuilder 类

SqlSessionFactory 的创建，需要使用 SqlSessionFactoryBuilder 对象的 build() 方法。由于 SqlSessionFactoryBuilder 对象在创建完工厂对象后，就完成了其历史使命，即可被销毁。所以，一般会将该 SqlSessionFactoryBuilder 对象创建为一个方法内的局部对象，方法结束，对象销毁。

其被重载的 build()方法较多：

![](/assets/Lusifer1514226217.png)

## SqlSessionFactory 接口

SqlSessionFactory 接口对象是一个重量级对象（系统开销大的对象），是线程安全的，所以一个应用只需要一个该对象即可。创建 SqlSession 需要使用 SqlSessionFactory 接口的的 openSession() 方法。

* openSession(true)：创建一个有自动提交功能的 SqlSession
* openSession(false)：创建一个非自动提交功能的 SqlSession，需手动提交
* openSession()：同 openSession(false)

## SqlSession 接口

SqlSession 接口对象用于执行持久化操作。一个 SqlSession 对应着一次数据库会话，一次会话以 SqlSession 对象的创建开始，以 SqlSession 对象的关闭结束。

SqlSession 接口对象是线程不安全的，所以每次数据库会话结束前，需要马上调用其 close() 方法，将其关闭。再次需要会话，再次创建。而在关闭时会判断当前的 SqlSession 是否被提交：若没有被提交，则会执行回滚后关闭；若已被提交，则直接将 SqlSession 关闭。所以，SqlSession 无需手工回滚。

SqlSession 接口常用的方法有：

![](/assets/Lusifer1514226440.png)

![](/assets/Lusifer1514226479.png)

![](/assets/Lusifer1514226540.png)

![](/assets/Lusifer1514226557.png)

![](/assets/Lusifer1514226581.png)

![](/assets/Lusifer1514226606.png)

![](/assets/Lusifer1514226624.png)


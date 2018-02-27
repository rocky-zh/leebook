# Log4j 常用布局类型

---

* org.apache.log4j.HTMLLayout：网页布局，以 HTML 表格形式布局
* org.apache.log4j.SimpleLayout：简单布局，包含日志信息的级别和信息字符串
* org.apache.log4j.PatternLayout：匹配器布局，可以灵活地指定布局模式。其主要是通过设置 PatternLayout 的 ConversionPattern 属性值来控制具体输出格式的 。

打印参数: Log4J 采用类似 C 语言中的 printf 函数的打印格式格式化日志信息

* **%m：**输出代码中指定的消息
* **%p：**输出优先级，即DEBUG，INFO，WARN，ERROR，FATAL 
* **%r：**输出自应用启动到输出该log信息耗费的毫秒数 
* **%c：**输出所属的类目，通常就是所在类的全名 
* **%t：**输出产生该日志事件的线程名 
* **%n：**输出一个回车换行符，Windows平台为“/r/n”，Unix平台为“/n” 
* **%d：**输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，比如：%d{yyy MMM dd HH:mm:ss , SSS}，输出类似：2002年10月18日  22 ： 10 ： 28 ， 921  
* **%l：**输出日志事件的发生位置，包括类目名、发生的线程，以及在代码中的行数。举例：Testlog4.main(TestLog4.java: 10 ) 
# java时间处理

## 六大时间处理类
+ java.util.Date   日期格式为: 年月日时分秒
+ java.sql.Date  日期格式为：年月日
+  java.sql.Time  日期格式为：时分秒 
+  java.sql.Timestamp 日期格式为：年月日时分秒纳秒（毫微秒）
    从数据库中取出来的日期一般都用getTimestamp()方法. 例如oracle中一个字段数据类型Date,要想获得准确日期就用getTimestamp()方法。
+ java.util.Calendar 抽象基类
+ java.text.SimpleDateFormat 其他五种均可以被格式化同种样式的时间

其他:
+ java.text.DateFormat
+ java.util.GregorianCalendar

### java.util.Date
+ 日期格式为: 年月日时分秒(精确到毫秒)
+ java.util.Date 就是在除了SQL语句的情况下面使用
+ getTime方法返回毫秒数


~~~java
public class TestDate {
	public static void main(String[] args) {
		Date date = new Date();
		System.out.println(date); // Mon Feb 12 09:27:57 CST 2018
	}
}
~~~


+ 与java.sql.Date转换
+ 使用Calendar进行日期和时间之间的转换
+ 使用  java.text.DateFormat/ java.text.SimpleDateFormat  类来格式化和分析日期字符串。
### java.sql.Date
+ 日期格式为：年月日. 只包含日期没有时间部分
+ getTime方法返回毫秒数
+ 与java.util.Date转换

### java.sql.Timestamp
### java.sql.Time
### java.util.Canlendar
+ 抽象类
+ Calendar 提供了一个类方法 getInstance，以获得此类型的一个通用的对象。


### SimpleDateFormat
+ 所有时间日期都可以被SimpleDateFormat格式化format()
+ parse方法：将字符串类型（java.lang.String）解析为日期类型（java.util.Date）
    format方法：将日期类型（Date）数据格式化为字符串（String）


###  java.text.DateFormat
+ 抽象类(SimpleDateFormat是它的子类)

### java.util.GregorianCalendar（Calendar的直接子类）
+ 提供了世界上大多数国家使用的标准日历系统。
> 在单一间断性的支持下同时支持儒略历和格里高利历系统，在默认情况下，它对应格里高利日历创立时的格里高利历日期（某些国家是在 1582 年 10 月 15 日创立，在其他国家要晚一些）。可由调用方通过调用 setGregorianChange() 来更改起始日期。

## 日期时间工具代码
### 定义日期时间格式
### 获取当前时间
### 记录当前时间及程序运行时长的方法


参考:
[1]:https://www.cnblogs.com/greatfish/p/6036567.html
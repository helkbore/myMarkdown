# java里的main方法
+ 形式
public static void main(String[] args){ }
public static void main(String args[]){ }

  String[] args可以写成String args[]，以及args的名称可以改变外
+ eclips快速创建
 输入`main`，在按住`Alt+/`的方式快速创建main方法。

+ static的定义是为了JVM在调用main方法时不用实例化对象，只需要在初始时加载main方法所在类，然后直接通过类名.main来调用main方法。
+ main的名称不能变是为了JVM能够识别程序运行的起点，main方法可以被重载，重载的main方法不会被执行。
+ 非守护线程
+ main()方法中可以throw Exception
# IDEA配置jdk-tomcat-web项目-servlet


-----------------------------------------------
## 简洁版顺序
1. 破解idea
2. 原始窗口 configure -> project defaults -> Project Struture
3. 原始窗口 configure -> settings -> Build, Execution, Deployment -> Application Servers
4. 新建project -> 空project(选择jdk和project language level 为8) -> 新建module 勾选web application -> OK
5. Project Struture -> facts -> 添加web.xml 注意修改WEB-INF的路径保存
6. 在文档结构里, WEB-INF下建classes和lib文件夹
7. 在Project Struture -> module -> paths中将 User module compile output path 都设为classes目录
8.  在Project Struture -> module -> dependencies -> 选jdk, 右边加号 -> 第一个 JARS or directories 确定后选 Jar Directory
9.  因为是web项目所以也在这个界面添加 roject Struture -> module -> dependencies -> +号 -> Libraries -> tomcat
10.  配置tomcat: run -> edit configurations -> +号 -> tomcat server -> local -> 给服务器命个名tomcat8.0.32 -> 下边tomcat server settings 里勾选 deploy applications configured in Tomcat instance 和 Preserve sessions across restarts and redeploys
11.  部署tomcat run -> edit configurations -> deployment -> +号 -> Artifacts -> 右边 application context 写上 /module名, 我的是 /MyTest
12.  修改index.jsp并运行
13.  在src下建包建servlet, 打开该文件, 在doGet方法里添加 `response.sendRedirect("error.jsp");`
14.  在web目录下建一个error.jsp并编辑
15.  修改web.xml配置servlet
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <servlet>
        <servlet-name>TestServlet</servlet-name>
        <servlet-class>test.web.TestServlet</servlet-class>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>TestServlet</servlet-name>
        <url-pattern>/test</url-pattern>
    </servlet-mapping>
</web-app>
```
16. 编译运行 地址为: localhost:8080/Module名/url-pattern中的地址
  我这里对应的就是: localhost:8080/MyTest/test
-----------------------------------------------


## 激活IDEA(见另一个文档)
## 配置JDK
1. 选择configure -> project defaults -> Project Struture(或者编辑器界面的file-> project struture)
![1](img/1.png)
2. 点击sdks -> 中间栏 加号, 选JDK 
3. 弹出一个对话框，让你去选择JDK安装的路径，选择好自己安装的JDK路径，点击OK即可。
![1](img/2.png)

## 配置tomcat(可跳过, 建完项目弄)
1. 原始界面(图1)中的configure -> settings -> Build, Execution, Deployment -> Application Servers
2. 点击中间栏的加号
3. 在tomcat Home中选择安装好的tomcat -> OK

## 创建web项目方法1
1. create new project (如果是编辑器界面则是: file -> new project)
2. 左边选择java, 右边project sdk选择对应的jdk, 下面点
![1](img/5.png)
3. (此种方法也可以不建module,将project当做module用了)右键点击项目-> new -> module
4. 勾选web application
![1](img/3.png)

## 创建web项目方法2
1. create new project (如果是编辑器界面则是: file -> new project)
2. 左边选择Empty project -> next -> finish
3. 建好project后会自动弹出一个建module的窗口
4. 中间栏加号 -> new module -> java ee 的web application
![1](img/3.png)
5. 我的因为2017.3版本, 没法选择建web.xml和版本号要之后建
6. 输入module名(IDEA里module意思是模块, 放在同一个project下, project类似于一个文件夹吧. 集合相关的模块, 这些模块也比较独立, 可以对应eclipse里的项目了) -> OK

### 打开project structure的两种方法:
1. File --> Project Structure（快捷键ctrl+alt+shift+s）
2. Navigation Bar中的Project Structure按钮
  ![1](img/7.png)
3. 没有这个Navigation Bar可在View中勾选Navigation Bar

## 2017.3版本创建web项目无法自动生成web.xml
1. FILE -> project struture -> 左边选fects
2. 中间栏选中web(新建的module名) 右边deployment descriptors面板里 +号选择web.xml及版本. -> OK
3. 注意: 选择web.xml的位置时将web-inf放入web目录下

![1](img/4.png)

## idea文档结构
+ idea文件夹和webapp.iml是IDEA自动创建的, 包含了工程和模块的配置数据
+ .idea和.settings分别是intelliJ和eclipse的相关依赖
+ build目录是class文件输出路径，这一点默认和eclipse是一样的。
+ out目录是所谓artifact输出路径，即提供给tomcat的web程序目录，具体的web根目录就是下级的web_test_Web_explorded。
+ src文件夹是源码目录
+ web文件夹相当于eclipse创建的web工程WebContent文件夹，包含了WEB-INF/web.xml及index.jsp
+ External Libraries包含了JDK及Tomcat带的jsp-api、servlet-api jar文件

## 完善工程目录
1. (方法一)添加WEB-INF/lib目录(注意这时候web-inf和web还是平级的,一会改)
  点击WEB-INF，右击New --> Directory，directory name填写lib，拷贝项目所需的jar包到此目录，右击lib目录 --> Add as Library
  ![1](img/6.png)
  这时会弹出Craete Library对话框，name填写lib即可，其它默认，点击OK确定
  添加完成可在Project Structure中的Libraries中看到
  注意：这种方法如果你不拷贝jar包到lib下，右击时是没有Add as Library选项的
2. (方法二)打开Project Structure --> libraries -> 点击+选择java --> 在弹出的Select Library Files中在WEB-INF下创建lib目录选择并点击OK --> 在弹出的Choose Categories of Selected Files中选择Jar Directory点击OK --> 在弹出的Choose Modules中点击OK

## 添加conf目录用于添加配置文件
### 方法一:
1. 右击项目New --> Directory --> directory name填写conf，点击OK --> 
2. 右击conf目录Mark Directory as --> Sources Root
3. 这样创建的conf source folder在Project Structure的Modules中可以看到

### 方法二：
1. 在Project Structure的左边Modules中, 右边sources 右击项目 --> New Folder --> Folder 
2. name填conf，点击OK --> 
3. 右击新建的conf --> Sources --> 点击底部的OK

## 配置classes和lib
### 在web-inf下建classes和lib
### 配置文件夹路径
1. File -> Project Structure (ctrl + shift + Alt + s) 或者使用工具栏的快捷键 -> 选择Modules -> 选择Paths -> 
2. 选择“Use module compile out path” -> 将Outputpath 和Test output path 都设置为刚刚创建的classes文件夹
3. 选择当前窗口的Dependencies -> 将Module SDK选择为1.6 ->点击右边的 + 号 -> 选择 “1 JARS or directories ...”
4. 选择刚刚创建的lib文件夹 -> OK

## 引入tomcat包(后面建servlet)
File -> Project Structure -> Modules -> 选择当前窗口的Dependencies -> +号 -> tomcat

## 配置tomcat容器
1. 打开菜单Run -> Edit Configurations...
2. 点击 “+” ，选择 “Tomcat Server” -> 选择“Local”
3. 在Name出输入新的服务器名，点击 "Application Server" 后面的 "Configure..."，弹出Application Servers窗口，在Tomcat Home 选择本地安装的tomcat目录 -> OK(如果前面已经配置了tomcat这步略)
4. "Run/Debug Configurations"窗口中Name一栏输入服务器的名字tomcat7，在“Server”面板中，勾选取消“After Launch”，设置“HTTP port”和“JMX port”（默认值即可），
5. 端口旁边的deploy applications configured in Tomcat instance 和 Preserve sessions across restarts and redeploys勾选上
6. 点击Apply -> OK，至此tomcat配置完毕（左边列表中tomcat图标上小红叉是未部署项目的提示，部署项目后就会消失）。

## 在tomcat上部署并运行项目
1. 在创建好tomcat后，可以通过工具栏快速打开tomcat的配置页面：
  也可以通过菜单栏：Run -> Edit Configurations... ->
  ![1](img/7.png)
2. 选择刚创建的tomcat7 -> 选择Deployment ->点击右边的“ + ”号 -> 选择 Artifact
3. 选择web项目 -> Application Context可以填“/module名”(也可以不填) -> Apply 
4. 编辑index.jsp, 运行tomcat

## 建servlet
1. 右键src -> new -> servlet 
2. 输入servlet名字, 包名
3. 打开servlet, 在doGet方法里写一个简单的跳转
  ```java
  response.sendRedirect("error.jsp");
  ```
4. 修改web.xml. 在web-app里添加
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

    <servlet>
        <servlet-name>TestServlet</servlet-name>
        <servlet-class>test.web.TestServlet</servlet-class>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>TestServlet</servlet-name>
        <url-pattern>/test</url-pattern>
    </servlet-mapping>
</web-app>
```
4. 编译运行 地址为: localhost:8080/Module名/url-pattern中的地址
  我这里对应的就是: localhost:8080/MyTest/test
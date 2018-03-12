# eclipse-maven安装配置java-web-servlet

> 系统说明: win7 64位


### 一. Maven安装
1. #### 环境 要求
	+ 看Maven下载说明也行
	+ jdk7.0以上
2.  #### 安装配置Maven
	+ 下载: Apache网站[ http://maven.apache.org/][1]   	
       Maven下载地址: [ http://maven.apache.org/download.cgi][2]
       备注: 直接下二进制的就行(源码版我没试) 解压到存放位置就行(像安装绿色软件那样, 注意不要有中文名等)
    + 配置环境变量
       我的电脑 -> 属性 -> 高级系统设置 -> 环境变量 -> 系统变量 -> 新建
       ```
       变量名:
       M2_HOME
       变量值:
       D:\install\maven\apache-maven-3.5.0
       ```
       
       配置系统环境变量`path` 添加(注意前后分号)
       ```
       ;M2_HOME;
       ```
       
       变量值为解压的maven文件夹所在地址
3.    #### 检查jdk和maven的环境变量配置是否成功
   dos命令行:
   ```dos
   mvn -v
   ```
4.    #### 配置`settings.xml`   conf文件夹下 添加阿里云镜像
  ```xml
  <mirrors>
	   <mirror>
		   <id>aliyun</id>
		   <name>aliyun Maven</name>
		   <mirrorOf>*</mirrorOf>
		   <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
	   </mirror>
   </mirrors>
```

5.   #### 修改本地仓库位置
	+ 选定文件夹位置如: `d:\repo`
	+ 修改`settings.xml`  55行左右
```xml
<localRepository>D:\repo</localRepository>
```
	+ 将conf下的`settings.xml`拷贝一份到本地仓库位置(两个地方的`settings.xml`都要修改)

### 二. eclipse中配置Maven
1. 依次点击  Window -> Preferences -> Maven -> Installations -> add  -> Directory: 找到并添加刚刚下载安装的maven(根目录即可, 即目录中有bin, conf等文件夹)

2.  Window -> Preferences -> Maven -> User Settings 手动配置Maven配置文件 (我的情况是点下面的ReIndex就自动加载进来了. 上面是Maven安装路径的, 下面是自定义本地仓库的Settings.xml)


### 三. 在eclipse中新建Maven项目

1. 鼠标右键 -> new -> other 直接在Wizards:下的文本框输入: Maven 找到: Maven Project 选中, 点击next
2. 直接点下一步 (抬头为: New Maven project Select project name and location)
3.  在表格中看第二列及`Artifact Id` 选择 `maven-archetype-webapp`(滚动条偏后)下面注释框显示: `

 ``` An archetype which contains a sample Maven WEbapp project```
 
4. 填入新建项目基本信息

   项目名|解释
   -------|-----|
   Group Id|项目的基本包名
   Artifact Id|项目名
   Version|默认就行
   Package|默认生成的包名, 不写也可以

5. 新生成路径解释(这4个包必须有且配置了, 右键项目 -> Build Path ->  Configure Build Path..  -> source.)
     建: src/main/java 右键项目 new -> Source Folder
6.  如果仍旧报错, 配置  右键项目 -> Build Path ->  Configure Build Path..  -> Library -> jdk -> edit -> 选择jdk路径  
    详情见:  [https://www.cnblogs.com/wbyp/p/7392681.html][8]

   文件名|Deploy Path|备注
   ------|-----
   src/main/resources|WEB-INF/classes|
   src/main/java|WEB-INF/classes|
   src/main/webapp|/|
   Maven Dependencies|WEB-INF/lib|
   src/test/resources|| 测试使用, 不需要部署
   src/test/java||测试使用, 不需要部署

7. 更改class路径 Java Build Path -> Source 每个文件夹下的output folder以及下面的, 双击选择targer/classes
8. 将项目转换成Dynamic Web Project 
   右键项目 -> properties -> project Facets(左边)  -> Convert to faceted from...(右边)

9.  运行项目, 看是否报错


### 四. 在Maven中新建Servlet

### 五. 如何在maven中央库查找包的地址

### 六. 其他方法 eclipse安装maven (未实践)

一般而言，新的eclipse都已经集成了maven，如果没有那么就安装，点击eclipse菜单栏Help->Eclipse Marketplace搜索关键字maven到插件Maven Integration for Eclipse 并点击安装即可，接下来将eclipse集成的maven换成我们自己的，而不用eclipse自带的，重新定位，点击Window -> Preference -> Maven -> Installation -> Add进行设置

### 七. maven中添加库
更新Maven项目。右键项目->Maven->Update Project
---------------------------------------------------------------------
在maven中央库查找包的地址。

https://mvnrepository.com 
参考: http://blog.csdn.net/u013181284/article/details/53203082?locationNum=13&fps=1  

      
      
[1]:  http://maven.apache.org/
[2]:  http://maven.apache.org/download.cgi
http://blog.csdn.net/u013181284/article/details/53203082?locationNum=13&fps=1
http://blog.csdn.net/u013181284/article/details/53203082?locationNum=13&fps=1 https://www.cnblogs.com/wbyp/p/7392681.html
https://www.cnblogs.com/teach/p/5906425.html
http://blog.csdn.net/mytt_10566/article/details/71774551?locationNum=2&fps=1
[8]: https://www.cnblogs.com/noteless/p/5213075.html
http://blog.csdn.net/zixiao217/article/details/53270692
http://blog.csdn.net/wild46cat/article/details/52467676
http://www.mobibrw.com/2013/685
https://www.cnblogs.com/noteless/p/5213075.html
http://www.cnblogs.com/youzhibing/p/5004619.html




# tomcat
## 安装
### 绿色版安装
1. 解压缩,路径不能有中文
2. 配置环境变量(实际上没设置就能用了= =!)
Path:
K:\green\apache-tomcat-8.0.32\bin
TOMCAT_HOME:
K:\green\apache-tomcat-8.0.32
CATALINA_HOME
K:\green\apache-tomcat-8.0.32
CATALINA_BASE
K:\green\apache-tomcat-8.0.32
CLASSPATH
K:\green\apache-tomcat-8.0.32\lib\servlet-api.jar
PATH:
K:\green\apache-tomcat-8.0.32\bin

// TODO
## 目录结构说明
### bin 
存放启动和关闭tomcat的脚本文件
### conf
存放tomcat服务器的各种配置文件
### lib
存放tomcat服务器和所有web应用
### logs
存放tomcat的日志文件
### temp
存放tomcat运行时产生的临时文件
### webapps
当发布web应用程序时, 通常把web应用程序的目录及文件放到work目录下
### works
tomcat讲jsp生成的servlet源文件和字节码文件放到这个目录下

## 配置部署项目的方法

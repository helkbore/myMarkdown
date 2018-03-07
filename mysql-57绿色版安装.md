# mysql-57绿色版安装
1. 环境变量:path和bin目录
  path
  J:\env\mysql-5.7.11-winx64\mysql-5.7.11-winx64\bin
2. 修改配置文件, 将my-default.ini 改为my.ini
```ini
[mysqld]
# 设置mysql的安装目录
basedir=J:\env\mysql-5.7.11-winx64\mysql-5.7.11-winx64
# 设置mysql数据库的数据的存放目录，必须是data
datadir=J:\env\mysql-5.7.11-winx64\mysql-5.7.11-winx64
# mysql端口
port=3306
character_set_server=utf8
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
# 忽略主机名的方式访问
skip-name-resolve
#忽略数据库表名大小写
lower_case_table_names=1
```
3. 安装mysql服务
 + cmd进入到mysql的bin目录.如果安装目录下存在“Data”目录，务必先删除data目录(或移动到其他地方)。
 + 执行:`mysqld --initialize`
 + 安装服务: (服务名随便起)
    - 自动
 ```
 mysqld --install [服务名]
 ```
    - 手动
     ```
 mysqld --install -manual [服务名]
 ```
 + 执行
 ```
 mysqld -install MySQL --defaults-file="J:\env\mysql-5.7.11-winx64\mysql-5.7.11-winx64\my.ini"
 ```
4. mysql服务启动与停止
 + 方法1
   在doc命令下进入到mysql的bin目录(D:\Dev\mysql-5.7.11\bin)，
   输入`net start mysql`启动mysql,
   输入`net stop mysql`停止mysql服务。
 + 方法2
 + 
    
https://www.cnblogs.com/weixiao520/p/4573619.html
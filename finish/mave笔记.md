# maven 笔记

## 一. 使用层面清单
1. 下载配置
2. 创建maven项目
3. eclipse maven
4. pom.xml添加包和手动添加依赖(src\lib)
5. 创建与导入
6. IDEA maven

## 二. 理解层面
### (1). 工程目录
配置项|默认值
-----|-----
source code | /src/main/java
resources   | /src/main/resources
Tests       | /src/test
Complied byte code | /target
distributable JAR  | /targer/classes


### groupId: 工作组, artifactId: 版本. 在仓库中这些属性是工程的唯一标识。
### (2). 生命周期
clean, default, site -- 相互独立的
#### clean 移除所有上一次构建生成的文件

1. pre 生命周期
2. 生命周期
3. post 生命周期  

运行post 生命周期名 会执行1, 2, 3步

#### site 生成项目站点文档并部署到特定的服务器上

### (3). 仓库
1. 本地仓库
2. 中央仓库 (maven社区管理的常用库)
3. 远程仓库
顺序 : 本地 -> 中央 -> 远程

### (4). 插件
Maven 实际上是一个依赖插件执行的框架, 插件主要用途
+ 创建 jar 文件
+ 创建 war 文件
+ 编译代码文件
+ 代码单元测试
+ 创建工程文档
+ 创建工程报告
在pom.xml用法
```xml
<project>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-antrun-plugin</artifactId>
                <version>1.1</version>
                <executions>
                    <execution>
                        <id>id.clean</id>
                        <phase>clean</phase>
                        <goals>
                            <goal>run</goal>
                        </goals>
                        <configuration>
                            <tasks>
                                <echo>clean phase</echo>
                            </tasks>
                        </configuration>
                    </execution>     
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

### (5). 创建工程
Maven 使用原型（archetype）插件创建工程。要创建一个简单的 Java 应用，我们将使用 maven-archetype-quickstart 插件。  

### (6). pom.xml解释
#### <dependency>
+ 外部依赖（library jar location）能够像其他依赖一样在 pom.xml 中配置。 
指定 groupId 为 library 的名称。 `<groupId>junit</groupId>`
指定 artifactId 为 library 的名称。   `<artifactId>junit</artifactId>`
指定作用域（scope）为系统。 `<scope>system</scope>`, ` <scope>test</scope>`
指定相对于工程位置的系统路径。 `<systemPath>${basedir}\src\lib\ldapjdk.jar</systemPath>`

#### 工程文档 `mvn site`
#### 管理依赖

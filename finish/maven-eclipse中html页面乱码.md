# maven-eclipse 中index.html页面乱码:
pox.xml修改:
```xml
<project>
    ……
    <properties>
        <argLine>-Dfile.encoding=UTF-8</argLine>
    </properties>
    ……
</project>
```
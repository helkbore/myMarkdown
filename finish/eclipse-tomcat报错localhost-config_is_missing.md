# eclipse-tomcat报错:The tomcat server configuration at /sever/tomcat v7.0 localhost-config is missing

+ 尝试重新配置tomcat
+ eclipse下的servers里的server.xml有重复的
```xml
<Context docBase="testaa" path="/testaa" reloadable="true" source="org.eclipse.jst.jee.server:testaa"/>
```



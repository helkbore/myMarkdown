# spring-security添加登录入口
## 一. 步骤
### 1. 在web.xml里添加 
```xml
<filter-mapping>
    <filter-name>springSecurityFilterChain</filter-name>
    <url-pattern>/card</url-pattern>
</filter-mapping>
```



### 2. 在app-security里添加
#### 2.1 (理论上放在哪里都行, 不必须被包含在哪些节点之下.但和原来的定义登录的部分放在一起比较好)
```xml
<bean id="cardLoginEntry" class="com.wsframe.platform.web.filter.RequestUriDirectUrlResolver">
    <property name="pattern" value="/card" />
    <property name="directUrl" value="/cardLogin.jsp" />
</bean>

```
其中`id`随意写, `class`照抄, `pattern`为请求的url以上配置对应如:`localhost/card`--- (`localhost`)为根目录

`directUrl`对应的是`jsp`存放的位置, 也可以是html文件

#### 2.2

在`<bean id="multipleAuthenticationLoginEntry"`的 `list`里添加`<ref bean="cardLoginEntry" />` 其中`cardLoginEntry` 对应上一步里随意起的`id`值

```xml
<bean id="multipleAuthenticationLoginEntry"    class="com.wsframe.platform.web.filter.MultipleAuthenticationLoginEntry">  
    <property name="defaultLoginUrl" value="/login.jsp"/>  
    <property name="directUrlResolvers">  
        <list>  
            <ref bean="mobileLoginEntry"/> 
            <!-- add by sun 2017-04-25 手机端 --> 
            <ref bean="weixinLoginEntry"/> 
            <!-- add by sun 2017-04-25 手机端 end-->
            
            <!-- add by gjj 2018-3-22 触摸屏 -->
            <ref bean="cardLoginEntry" />
            <!-- add by gjj 2018-3-22 触摸屏 end -->
        </list>  
    </property>  
</bean> 
```

3. 添加不拦截的页面, 还是在app-security, 添加一个value, 即: `<value>/cardLogin.jsp</value>`和 `<value>/card/login.ws</value>`
```xml
<bean id="securityMetadataSource"
    class="com.wsframe.platform.web.filter.WsSecurityMetadataSource" scope="singleton" >
    
    <property name="anonymousUrls">
        <set>
            <value>/mobileLogin.jsp</value>
            <value>/mobileLogin.ws</value>
```
## 二. 分析原理
1. web.xml添加过滤器, 个人理解就是输入的路由得走进spring-security
```xml
<filter-mapping>
    <filter-name>springSecurityFilterChain</filter-name>
    <url-pattern>/card</url-pattern>
</filter-mapping>
```

2. app-security(在spring的配置文件引入的, spring的配置文件是在web.xml中引入的)
这个xml中观察代码发现是自定义的过滤器
```xml
<!--登录入口定义-->
<bean id="multipleAuthenticationLoginEntry"    class="com.wsframe.platform.web.filter.MultipleAuthenticationLoginEntry">  
    <property name="defaultLoginUrl" value="/login.jsp"/>  
    <property name="directUrlResolvers">  
        <list>  
            <ref bean="mobileLoginEntry"/> 
            <ref bean="weixinLoginEntry"/> 
            <ref bean="cardLoginEntry" />
        </list>  
    </property>  
</bean>  
```
处理的类是`com.wsframe.platform.web.filter.MultipleAuthenticationLoginEntry`
代码有
```java
private List<DirectUrlResolver> directUrlResolvers = new ArrayList<DirectUrlResolver>(); 

public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {  
    
    String ctxPath = request.getContextPath();
    for (DirectUrlResolver directUrlResolver : directUrlResolvers) {  
        if (directUrlResolver.support(request)) {  
            String loginUrl = directUrlResolver.directUrl();  
            response.sendRedirect(ctxPath+ loginUrl);  
            return;  
        }  
    }  
    response.sendRedirect(ctxPath+defaultLoginUrl);  
}  
```

而DirectUrlResolver的实现是
```java
public class RequestUriDirectUrlResolver extends AbstractDirectUrlResolver {  
	
	/**
	 * 获得跳转路径里的标识
	 */
    @Override  
    public boolean support(HttpServletRequest request) {
    	
        String requestURI = request.getRequestURI();  
        return requestURI.contains(this.pattern);
        
    }
    
}  
```

### 个人理解
在xml中注册了之后, 一个url进来以后, 大致经历这几个文件  
> web.xml -> app.content.xml -> app-security.xml
-> 过滤器 MultipleAuthenticationLoginEntry -> RequestUriDirectUrlResolver的support方法

文件名|解释
-----|----
web.xml|配置url的过滤器, 符合规则则将其交给spring , -> spring security处理
app-security |配置了自定义的过滤器, 即: MultipleAuthenticationLoginEntry
MultipleAuthenticationLoginEntry|接收到url的request请求, 循环遍历RequestUriDirectUrlResolver的数组调用每个RequestUriDirectUrlResolver对象的support方法(即app-security中注册的). 如果请求在其中的某个对象中, 则允许跳转
RequestUriDirectUrlResolver|读取url的request请求, 与pattern(app-security中配置的pattern属性)比对, 如果存在则返回给过滤器: true -> 过滤器跳转

下面这段代码: app-security
第二个bean的id对应 上面`<ref bean="cardLoginEntry" />` 即将RequestUriDirectUrlResolver格式的匹配字典关联到了过滤器(MultipleAuthenticationLoginEntry)上
```xml
<bean id="multipleAuthenticationLoginEntry"    class="com.wsframe.platform.web.filter.MultipleAuthenticationLoginEntry">  
    <property name="defaultLoginUrl" value="/login.jsp"/>  
    <property name="directUrlResolvers">  
        <list>  
            <ref bean="cardLoginEntry" />
        </list>  
    </property>  
</bean>  
    
<bean id="cardLoginEntry" class="com.wsframe.platform.web.filter.RequestUriDirectUrlResolver">
    <property name="pattern" value="/card" />
    <property name="directUrl" value="/cardLogin.jsp" />
</bean>
```

# 维森框架培训-ssm
> 整理人: 郭嘉婧 2018年3月8日

## controller

### 定义声明
1. 类注解-表名角色(controller)
 ```java
 @Controller
 @RequestMapping("/appsystem/wzxm/menu/menu")
 public class MenuController extends BaseController{
 ```
2. 类注解-跳转路径
  依赖注入: 建立一个对象池, tomcat启动的时候
3. 继承父类(BaseController)
```java
 import com.wsframe.core.web.controller.BaseController;
```
4. 成员变量 (实例化的类)
```java
 @Resource
 private MenuService menuService;
```
5. 方法注解-跳转入口
  ```java
  @RequestMapping("list")
  // @ResponseBody
	public ModelAndView list(HttpServletRequest request, HttpServletResponse response) throws Exception {
  ```
6. 方法注解-其他
  @ResponseBody: json格式输出
7. 方法参数默认有: HttpServletRequest request, HttpServletResponse response

// TODO: ajax注解


### 获取参数(详见RequestUtil类)
```java
int i = RequestUtil.getInt(request, "Q_xxxx_S", 0);
```
### 调用service
```java
List<Menu> list = menuService.getAll(new QueryFilter(request,"menuItem"));
```

```java
Menu menu = menuService.getById(id);
```

```java
menuService.delById(id);
```

```java
menuService.save(menu);
```


### 跳转视图(view层)
```java
ModelAndView mv = this.getAutoView();
mv.addObject("menulist", list);
return mv ;
```

```java
// 重定向方式
ResultMessage message = null;
String preUrl= RequestUtil.getPrePage(request);
try {
	// 代码略
    message = new ResultMessage(ResultMessage.Success, "删除成功!");
}catch(Exception e) {
    message = new ResultMessage(ResultMessage.Fail, "删除失败" + e.getMessage());
}
addMessage(message, request);
response.sendRedirect(preUrl);
```

```java
// ajax-json方式(个人猜测)
String resultMsg=null;
try{
    writeResultMessage(response.getWriter(),resultMsg,ResultMessage.Success);
} catch(Exception e) {
    writeResultMessage(response.getWriter(),resultMsg+","+e.getMessage(),ResultMessage.Fail);
}
```
### 备注
1. 同实体类下的service以类成员方式实例化(见 定义声明-4. 成员变量)
2. QueryFilter类是jsp后台管理界面表格列表的一些插件.
3. 路径路由问题: URL -> controller -> 默认jsp页
  首页地址(根路径):  
  > localhost:8080/oa  
  
    访问地址为: 
    > localhost:8080/oa/<span style="color: LightSeaGreen">/appsystem/wzxm/menu/menu/</span>/<span style="color: Tomato"> list</span>.ws
 
 controller类路径为:  
 > <span style="color: LightSeaGreen">/appsystem/wzxm/menu/menu/</span>
 
 controller方法路径为:
 > <span style="color: Tomato"> list</span>
 
 该controller默认jsp页: 
 > /WebContent/WEB-INF/view<span style="color: LightSeaGreen">/appsystem/wzxm/menu/menu</span><span style="color: Tomato">List.jsp</span>


### 其他方法
### RequestUtil类
```java
Long id=RequestUtil.getLong(request, "id", 0);
int i = RequestUtil.getInt(request, "Q_xxxx_S", 0);
Long[]  lAryId=RequestUtil.getLongAryByStr(request,"id");
String str = RequestUtil.getString(request, "name");
String ip = RequestUtil.getIpAddr(request);
String errorUrl = RequestUtil.getErrorUrl(request);
boolean isAdmin = RequestUtil.getBoolean(request, "isAdmin", false);
short status = RequestUtil.getShort(request, "status", (short)-1);
Map<?, ?> paraMap = RequestUtil.getParameterValueMap(request, false, false);
sysAudit.setReqParams(RequestUtil.getRequestInfo(request).toString());
map.putAll(RequestUtil.getQueryMap(request));
Date startTime = RequestUtil.getDate(request, "startTime", StringPool.DATE_FORMAT_DATE);
String[] type = RequestUtil.getStringAryByStr(request, "type");
String preUrl= RequestUtil.getPrePage(request);
response.sendRedirect(preUrl);
```


## service
### 定义
```java
@Service
public class MenuService extends BaseService<Menu>{}
```

## dao
### 定义
```java
@Repository
public class MenuDao extends BaseDao<Menu>{
```
## map.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd"> 
<mapper namespace="com.appsystem.wzxm.menu.model.Menu">
	<resultMap id="Menu" type="com.appsystem.wzxm.menu.model.Menu">
		<id property="id" column="ID" jdbcType="NUMERIC"/>
		<result property="pid" column="PID" jdbcType="NUMERIC"/>
		<result property="title" column="TITLE" jdbcType="VARCHAR"/>
		<result property="createDate" column="CREATE_DATE" jdbcType="DATE"/>
		<result property="username" column="USERNAME" jdbcType="VARCHAR"/>
		<result property="bak" column="BAK" jdbcType="VARCHAR"/>
	</resultMap>

	<sql id="columns">
		ID,PID,TITLE,CREATE_DATE,USERNAME,BAK
	</sql>
	
	<sql id="dynamicWhere">
		<where>
			<if test="@Ognl@isNotEmpty(id)"> AND ID= #{id}</if>
			<if test="@Ognl@isNotEmpty(title)"> AND TITLE  LIKE '%${title}%'  </if>
		</where>
	</sql>

	<insert id="add" parameterType="com.appsystem.wzxm.menu.model.Menu">
		INSERT INTO MENU
		(ID, PID, TITLE, CREATE_DATE, USERNAME, BAK)
		VALUES
		(#{id,jdbcType=NUMERIC},
		#{pid,jdbcType=NUMERIC}, #{title,jdbcType=VARCHAR}, #{createDate,jdbcType=DATE}, #{username,jdbcType=VARCHAR}, #{bak,jdbcType=VARCHAR})
	</insert>
	
	<delete id="delById" parameterType="java.lang.Long">
		DELETE FROM MENU 
		WHERE
		ID=#{id}
	</delete>
	
	<update id="update" parameterType="com.appsystem.wzxm.menu.model.Menu">
		UPDATE MENU SET
		PID=#{pid,jdbcType=NUMERIC},
		TITLE=#{title,jdbcType=VARCHAR},
		CREATE_DATE=#{createDate,jdbcType=DATE},
		USERNAME=#{username,jdbcType=VARCHAR},
		BAK=#{bak,jdbcType=VARCHAR}
		WHERE
		ID=#{id}
	</update>
	
		    
	<select id="getById" parameterType="java.lang.Long" resultMap="Menu">
		SELECT <include refid="columns"/>
		FROM MENU
		WHERE
		ID=#{id}
	</select>
	
	<select id="getAll" resultMap="Menu">
		SELECT <include refid="columns"/>
		FROM MENU   
		<include refid="dynamicWhere" />
		<if test="@Ognl@isNotEmpty(orderField)">
		order by ${orderField} ${orderSeq}
		</if>
		<if test="@Ognl@isEmpty(orderField)">
		order by ID  asc
		</if>
	</select>
</mapper>

```
## 解释说明

备注: 大写字母一般是数据库字段名

### 结果集映射
备注: 定义数据库列 与 实体类之间的映射(查询结果转实体类以便java操作)

```xml
<resultMap id="Menu" type="com.appsystem.wzxm.menu.model.Menu">
    <id property="id" column="ID" jdbcType="NUMERIC"/>
    <result property="pid" column="PID" jdbcType="NUMERIC"/>
    <result property="title" column="TITLE" jdbcType="VARCHAR"/>
    <result property="createDate" column="CREATE_DATE" jdbcType="DATE"/>
    <result property="username" column="USERNAME" jdbcType="VARCHAR"/>
    <result property="bak" column="BAK" jdbcType="VARCHAR"/>
</resultMap>
```
### 查询字段集合
备注: 查询语句时 查询的字段定义一个集合. 
目的 :简便通用(重复使用)不易错, 比 `select *`要有效率

```xml
<sql id="columns">
    ID,PID,TITLE,CREATE_DATE,USERNAME,BAK
</sql>
```

### 定义查询条件
备注: 查询语句时的查询条件集合 
目的: 简便通用(重复使用)不易错, 框架进一步验证了非空等条件, 安全性高

```xml
<sql id="dynamicWhere">
    <where>
        <if test="@Ognl@isNotEmpty(id)"> AND ID= #{id}</if>
        <if test="@Ognl@isNotEmpty(title)"> AND TITLE  LIKE '%${title}%'  </if>
    </where>
</sql>
```

### 增删改查操作的语句
备注: 
+ id属性的值即为dao可调用的方法名. 
+ parameterType属性值为调用方法时传入的参数类型(类似于定义方法时定义的参数类型. 如果调用实体类由于没有import语句, 所以要写类的全称-即包含包名), 必要的时候要使用包装类(见下面代码段2-delById)
+ 示例代码3-getAll应用到前面的结果集, 查询字段集, 查询条件集
+ `<if test="@Ognl@isNotEmpty(orderField)">order by ${orderField} ${orderSeq}</if>` 通常是给框架使用的, 也可以通过页面传参的方式排序
+ 可以按照这种方式定义用户自己的查询方法, 用dao调用.
+ 易错点: 实体类属性名与数据库字段名写混淆,  多个字段定义时多写了`,` 
+ 建议: 弄清楚哪部分是实体类哪部分是数据库字段, 将数据库字段和SQL语句用大写来写.
+ 建议: 用可执行的SQL语句修改xml定义

```xml
<insert id="add" parameterType="com.appsystem.wzxm.menu.model.Menu">
    INSERT INTO MENU
    (ID, PID, TITLE, CREATE_DATE, USERNAME, BAK)
    VALUES
    (#{id,jdbcType=NUMERIC},
    #{pid,jdbcType=NUMERIC}, #{title,jdbcType=VARCHAR}, #{createDate,jdbcType=DATE}, #{username,jdbcType=VARCHAR}, #{bak,jdbcType=VARCHAR})
</insert>
```

```xml
<delete id="delById" parameterType="java.lang.Long">
    DELETE FROM MENU 
    WHERE
    ID=#{id}
</delete>
```

```xml
<select id="getAll" resultMap="Menu">
    SELECT <include refid="columns"/>
    FROM MENU   
    <include refid="dynamicWhere" />
    <if test="@Ognl@isNotEmpty(orderField)">
    order by ${orderField} ${orderSeq}
    </if>
    <if test="@Ognl@isEmpty(orderField)">
    order by ID  asc
    </if>
</select>
```


## jsp
#### 1. 查询按钮等
```xml
<div class="panel-toolbar">
    <div class="toolBar">
        <div class="group"><a class="link search" id="btnSearch"><span></span>查询</a></div>
        <div class="l-bar-separator"></div>
        <div class="group"><a class="link add" href="edit.ws"><span></span>添加</a></div>
        <div class="l-bar-separator"></div>
        <div class="group"><a class="link update" id="btnUpd" action="edit.ws"><span></span>修改</a></div>
        <div class="l-bar-separator"></div>
        <div class="group"><a class="link del"  action="del.ws"><span></span>删除</a></div>
    </div>	
</div>
```

备注: `<a class="link search"` 该类用js装载了查询按钮功能

---------------------------------------------------------------

#### 2. 查询提交的表单
```xml
<div class="panel-search">
	<form id="searchForm" method="post" action="list.ws">
		<div class="row">
			<span class="label">菜单名称:</span><input type="text" name="Q_title_S"  class="inputText" />
		</div>
	</form>
</div>
```

备注: `<form id="searchForm"`该id用js装载了查询表单功能

---------------------------------------------------------------


#### 3. 表格
```xml
<display:table name="menulist"  id="menuItem" requestURI="list.ws" sort="external" cellpadding="1" cellspacing="1" class="table-grid">
    <display:column title="${checkAll}" media="html" style="width:30px;">
        <input type="checkbox" class="pk" name="id" value="${menuItem.id}">
    </display:column>
    <display:column property="id" title="id" sortable="true" sortName="id"></display:column>
    <display:column property="pid" title="父id" sortable="true" sortName="pid"></display:column>
    <display:column property="title" title="标题" sortable="true" sortName="title"></display:column>
    <display:column property="username" title="操作人" sortable="true" sortName="username"></display:column>
    <display:column  title="创建日期" sortable="true" sortName="createDate">
        <fmt:formatDate value="${menuItem.createDate}" pattern="yyyy-MM-dd" />
    </display:column>
    <display:column property="bak" title="备注" sortable="true" sortName="bak"></display:column>
    <display:column  title="操作" >
        <a  href="del.ws?id=${menuItem.id}">删除</a>
    </display:column>
</display:table>
```

备注:
sortable="true": 是否允许此列进行排序
sortName: 排序的字段 (对应数据库字段名)

---------------------------------------------------------------


#### 4. 分页代码
```xml
<wsframe:paging tableId="menuItem"/>
```

#### 5. 编辑添加页判断是编辑还是添加
```xml
<c:choose>
 <c:when test="${not empty proBudgettypeItem.id}">
     <span class="tbar-label"><span></span>修改菜单</span>
 </c:when>
 <c:otherwise>
     <span class="tbar-label"><span></span>添加菜单</span>
 </c:otherwise>	
</c:choose>
```


#### 6. 添加编辑页表单
```xml
<!-- 菜单名称 -->
<tr>
<td align="right" style="width:15%;" class="formTitle" nowrap="nowarp">菜单名称: </td>
<td style="width:35%;" class="formInput">
	<span name="editable-input" style="display:inline-block;" isflag="tableflag">
		<input type="text" name="m:menu:title" lablename="菜单名称"  value="${menu.title}" />
	</span>
</td>
</tr>
```

#### 7. hidden标签
```xml
<input type="hidden" name="m:menu:id" value="${menu.id}"/>
```

### 备注
1. 在代码段3里, 操作那一列 要在超级链接中传入id, 用${menu.id}是不行的, 在表格里${menu.属性}无法显示(至少无法输出, 要用QueryFilter()命名的即: ${menuItem.id}
2. 通过url传参直接对list列表进行排序(对应后面的map.xml) 
>  http://localhost:8080/oa/appsystem/wzxm/menu/menu/list.ws?orderField=title&orderSeq=desc
# JAVA-MVC各层命名规则-对象术语
+ DAO (data access object) 数据库访问对象
  访问数据库的
+ DTO (Data Transfer Object) 数据传输对象      
  DTO 是一组需要跨进程或网络边界传输的聚合数据的简单容器。它不应该包含业务逻辑，并将其行为限制为诸如内部一致性检查和基本验证之类的活动。注意，不要因实 现这些方法而导致 DTO 依赖于任何新类。在设计数据传输对象时，您有两种主要选择：使用一般集合；或使用显式的 getter 和 setter 方法创建自定义对象。
  个人感觉是api类似那种的用法
+ DO (Domain Object) 领域对象
  从现实世界中抽象出来的有形或无形的业务实体。
  概念模型, 描述逻辑为基础, 可以包含行为和数据

+ BO (business object) 业务对象
+ POJO (plain ordinary java object) 简单无规则java对象
+ PO (persistant object) 持久对象
  就是在Object/Relation Mapping框架中的Entity
  接收数据库DAO层的(java bean???)
+ VO(value object/ view object) 值对象/表现层对象
+ TO  (Transfer Object) 传输对象  在应用程序不同 tie( 关系 ) 之间传输的对象

JSP/HTML -> VO -> PO -> DAO -> DB -> PO -> BO -> VO -> JSP/HTML
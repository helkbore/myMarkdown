# web.xml文件详解

## web.xml主要用来配置web工程, 但web.xml不是必须的
## web容器的加载顺序
> ServletContext -> context-param -> listener -> filter -> servlet
1. 启动一个 WEB 项目的时候，WEB 容器会去读取它的配置文件 `web.xml`，读取 `<listener>` 和 `<context-param>` 两个结点。
2. 创建一个 `ServletContext` （ servlet 上下文），这个 web 项目的所有部分都将共享这个上下文。
3. 将 `<context-param>` 转换为键值对，并交给 `servletContext`。  
4. 创建 `<listener>` 中的类实例，创建监听器。 
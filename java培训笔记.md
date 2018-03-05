# JAVA基础
## 基础语法
### 数据类型(java内存 堆栈)
#### 基本数据类型

> 引用数据类型

`
> java内存 堆栈

`int`与`double` 用的比较多
 > `id` 一般用 `long`

#### 引用数据类型


### Java关键字

<table>
	
	<tr>
		<td>class</td>
		<td>类名和文件名时一致的</td>
	</tr>
	
	<tr>
		<td>native</td>
		<td>本地, 原生方法(非java语言)</td>
	</tr>
	<tr>
		<td>static</td>
		<td>
			1. 不用new对象
			2. 内存
		</td>
	</tr>
	<tr>
		<td>synchronized</td>
		<td>线程, 一个对象调用该对象时, 其他的对象不能再调用</td>
		<td>缺点: 不能并发, 效率低, 并发效果低</td>
	</tr>
	<tr>
		<td>transient</td>
		<td>线程高并发相关</td>
	</tr>
	
</table>

### 构造方法
#### 私有构造方法
单例模式: 一般IO流
#### 公有构造方法
+ 无参构造方法
+ 有参构造方法

`
##### 方法的重载和重写
> 重载: 方法名一样, 参数不同(参数的个数, 参数的类型)

> 重写: 重写父类的方法, 方法名, 参数一样.

`

### 包装类

`int`与`Integer`

+ int有默认值, 包装类没有默认值
+ 包装类有方法有属性
+ 比较大小
      <pre>`i.equals(args0)`</pre>
+ 转换
	<pre>Integer.valueOf(s)</pre>


### 集合
* List (序列)
* Map (键值对)
* Set (不知道Map里的键名情况下-遍历map)


> HashMap 非线程安全, 性能较高

> HashTable 线程安全, 性能效率低

    Map<String, String> map = new HashMap<String><String>();
	map.put("username", "szg");


> 如果不使用泛型: 在`map.put("username", "szg");`部分进行强制类型转换也行, 但不推荐


	Set s = map.keySet();

循环set

	for(String str : s) {

	}

遍历list 
<pre>
for(int i = 0; i < list.size(); i++) {}
</pre>


	List<String> list
		for (String[] s : list) {
	
	} 

> 第二种方法效率相对高
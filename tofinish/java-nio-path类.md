# java-nio-path类
## 所在包: java.nio.file.path, java.nio.file.paths
## 方法
+ path是接口 , paths是类

# 创建实例的方法: 

```java
package IOtest;

import java.nio.file.Path;
import java.nio.file.Paths;

public class PathTest {
	public static void main(String[] args) {
		// 实例化: 使用get()工厂方法获取实例
		Path path = Paths.get("D:\\wsframe\\workspace\\MyTest");
		Path path2 = Paths.get("d:\\data\\projects\\a-project\\..\\another-project");
		
		// 路径标准化
		Path nPath = path.normalize();
		Path nPaht2 = path2.normalize();
		
		System.out.println(path2);
		System.out.println(nPaht2);
	}
}


```

参考:
https://www.jianshu.com/p/17b8e042a90b
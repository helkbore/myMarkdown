```java
package baseTest;

public class BaseDataStructer {

	public static void main(String[] args) {
		Long l1 = 10000026500083l;
		Long l2 = 10000026500083l;
		
		if  (l1 == l2 ) {
			System.out.println("== ");
		}
		
		if (l1.equals(l2)) {
			System.out.println("equals");
		}
		
		if (l1.longValue() == l2.longValue()) {
			System.out.println("longValue");
		}
		
		
		
	}
}

```

> 输出结果是:
> equals
> longValue


# java-基础-集合框架

## 集合框架定义的接口
+ Collection
+ List
+ Set
+ SortedSet
+ Map
+ Map.Entry
+ SortedMap
+ Enumeration

### Collection
### List
+ 继承于Collection和一个 List实例存储一个有序集合的元素。

### Set
+ 继承于 Collection，是一个不包含重复元素的集合。

### SortedSet
+ 继承于Set保存有序的集合。

### Map
+ 将唯一的键映射到值。


###  Map.Entry
+ 描述在一个Map中的一个元素（键/值对）。是一个Map的内部类。
+ 由Map接口中声明的entrySet()方法返回一个包含映射条目的集。每个组元素都是一个Map.Entry对象。

### SortedMap
+ 继承于Map，使Key保持在升序排列。

### Enumeration
+ 这是一个传统的接口和定义的方法，通过它可以枚举（一次获得一个）对象集合中的元素。这个传统接口已被迭代器取代。
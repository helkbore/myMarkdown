# java-基础-流和文件



[TOC]







## IO
+ java.io 是流IO
+ java.nio是块IO (New IO), Non-blocking IO（非阻塞IO）
+ 流IO的好处是简单易用，缺点是效率较低。块IO效率很高，但编程比较复杂。


## 流

### 流的概念





### 流的分类

+ 按照处理数据大小: 字节流和字符流

+ 按照流的方向：输入流和输出流

+ 按照功能分为：分为节点流和处理流







#### 字节流和字符流

##### 字节流 

+ 读取的数据以字节为单位（byte），8bit

+ 抽象类: InputStream, OutputStream

+ 使用情况: 当我们读取文件然后在写入到其它的文件时候，不用过多的关注读取的内容时，通常使用的是字节流

+ 相当于是在处理二进制文件

+ 读取数据效率高

+ 保持数据的完整性。





##### 字符流

+ 读取的数据以字符为单位（char），16bit

+ 抽象类: InputReader,OutputReader

+ 使用情况: 当我们读取文件的内容，并要对文件的内容进行加工时（修改，判断等），通常是使用的字符流

+ 文件的最基本单位是字符，同样保持数据的完整性。



#### 输入流和输出流

**输入流**：是指从流读入数据。

**输出流**：是指向流写出数据。



#### 处理流和节点流

##### 节点流

节点流从一个特定的数据源读写数据。即节点流是直接操作文件，网络等的流，例如FileInputStream和FileOutputStream，他们直接从文件中读取或往文件中写入字节流。

##### 处理流

“连接”在已存在的流（节点流或处理流）之上通过对数据的处理为程序提供更为强大的读写功能。过滤流是使用一个已经存在的输入流或输出流连接创建的，过滤流就是对节点流进行一系列的包装。例如BufferedInputStream和BufferedOutputStream，使用已经存在的节点流来构造，提供带缓冲的读写，提高了读写的效率，以及DataInputStream和DataOutputStream，使用已经存在的节点流来构造，提供了读写Java中的基本数据类型的功能。他们都属于过滤流。



#### 常见流类介绍

##### 常见节点流类型

对文件操作的字符流有FileReader/FileWriter，字节流有FileInputStream/FileOutputStream



##### 常见处理流类型

###### 缓冲流
+ 缓冲流要“套接”在相应的节点流之上，对读写的数据提供了缓冲的功能，提高了读写效率，同时增加了一些新的方法。
+ 字节缓冲流有BufferedInputStream/BufferedOutputStream，字符缓冲流有BufferedReader/BufferedWriter
+ 字符缓冲流分别提供了读取和写入一行的方法ReadLine和NewLine方法。
+ 对于输出地缓冲流，写出的数据，会先写入到内存中，再使用flush方法将内存中的数据刷到硬盘。所以，在使用字符缓冲流的时候，一定要先flush，然后再close，避免数据丢失。

######转换流
+ 用于字节数据到字符数据之间的转换。
+ 仅有字符流InputStreamReader/OutputStreamWriter。其中，InputStreamReader需要与InputStream“套接”，OutputStreamWriter需要与OutputStream“套接”。

######数据流
+ 提供了读写Java中的基本数据类型的功能。
+ DataInputStream和DataOutputStream分别继承自InputStream和OutputStream，需要“套接”在InputStream和OutputStream类型的节点流之上。

######对象流
+ 用于直接将对象写入写出。
+ 流类有ObjectInputStream和ObjectOutputStream，本身这两个方法没什么，但是其要写出的对象有要求，该对象必须实现Serializable接口，来声明其是可以序列化的。否则，不能用对象流读写。
+ 还有一个关键字比较重要，transient，由于修饰实现了Serializable接口的类内的属性，被该修饰符修饰的属性，在以对象流的方式输出的时候，该字段会被忽略。


### 流的操作规律

1. 明确源和目的。 数据源：就是需要读取，可以使用两个体系：InputStream、Reader； 数据汇：就是需要写入，可以使用两个体系：

2. 操作的数据是否是纯文本数据？ 如果是：数据源：Reader 数据汇：Writer 如果不是：数据源：InputStream 数据汇：OutputStream

3. 虽然确定了一个体系，但是该体系中有太多的对象，到底用哪个呢？ 明确操作的数据设备。 数据源对应的设备：硬盘(File)，内存(数组)，键盘(System.in) 数据汇对应的设备：硬盘(File)，内存(数组)，控制台(System.out)

4. 需要在基本操作上附加其他功能吗？比如缓冲，如果需要就进行装饰







## java.io包

### 四个基类

+ InputStream/Reader：所有输入流的基类，前者是输入字节流，后者是输入字符。

+ OutputStream/Writer：所有输出流的基类，前者是输出字节流，后者是输出字符流

### java io分类
+ 标准输入输出
+ 文件的操作
+ 网络上的数据流
+ 字符串流
+ 对象流
+ zip文件流

### IO包

+ FileInputStream/FileOutputStream  读/写文件

+ BufferedReader/BufferedWriter 读写文本文件

+ DataOutputStream/DataInputStream 二进制文件





## 代码

### 控制台输入输出


### 文件读写

```java

    // 节点流和处理流 文件读写

	public void test2() throws IOException {

		// 节点流FileOutputStream直接以A.txt(项目根目录)作为数据源操作

		FileOutputStream fileOutputStream = new FileOutputStream("A.txt");

		

		// 过滤流BufferedOutputStream进一步装饰节点流，提供缓冲写

		BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(fileOutputStream);

		

		// 过滤流DataOutputStream进一步装饰过滤流，使其提供基本数据类型的写

		DataOutputStream out = new DataOutputStream(bufferedOutputStream);

		

		out.writeInt(3);

		out.writeBoolean(true);

		out.flush();

		out.close();

		

		// 此处输入节点流，过滤流正好跟上边输出对应

		DataInputStream in = new DataInputStream(new BufferedInputStream(new FileInputStream("A.txt")));

		System.out.println(in.readInt());

		System.out.println(in.readBoolean());

		in.close();

		

	}

```



## 常用工具代码

### 文件拷贝

https://www.w3cschool.cn/jeep711blog/jeep711blog-tlif28ho.html

## 面试题相关
## 参考
挺多挺详尽的, 但评论说有错误? : http://blog.csdn.net/hguisu/article/details/7418161
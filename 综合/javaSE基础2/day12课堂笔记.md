# 学习目标

- 能够阐述编码表的意义				


- 能够使用转换流读取指定编码的文本文件
- 能够使用转换流写入指定编码的文本文件
- 能够使用Properties的load方法加载文件中配置信息(后面的内容)
   能够使用Properties的store方法保存配置信息到文件(后面的内容)	
- 能够说出打印流的特点
- 能够使用序列化流写出对象到文件
- 能够使用反序列化流读取文件到程序中
- 能够导入commons-io工具包
- 能够配置classpath添加jar包到项目中
- 能够使用FileUtils常用方法操作文件	

# 今天知识

- 转换流
- 对象流
- 打印流
- 第三方工具







## 1. 转换流引入
### 1.1 代码演示乱码问题	

```java
public class OutputStreamWriterDemo01 {
	public static void main(String[] args) throws Exception{
		// 创建字符输入流
		FileReader fr = new  FileReader("a.txt");
		// 一次读取一个字符数组
		char[] buf = new char[1024];
		// 返回实际读取的字符个数
		int len = fr.read(buf);
		System.out.println(new String(buf,0,len));
		// 关闭流
		fr.close();
	}
}
```

- 乱码问题分析图

![](/Users/pkxing/Documents/82期就业班/day12/笔记/FileReader读取文件乱码原因分析图.png)

### 1.2 为什么要使用转换流

- FileReader和BufferedReader存在的问题
  - 只能使用默认的编码GBK读取文件的中数据，如果文件的内容不是以GBK方法编码，则会出现乱码问题。
- 修改读取文件时使用的编码，只有转换流才具备修改码表的能力。

### 1.3 转换流的作用

- 字节流和字符流相互转换的桥梁。

## 2. 编码表概述

```css
在计算机中无论任何数据的传输、存储、持久化，都是以二进制的形式体现的。
那么当我存一个字符的时候，计算机需要持久化到硬盘，或者保存在内存中。
这个时候保存在内存、硬盘的数据显然也是二进制的。
那么当我需要从硬盘、内存中取出这些字符，再显示的时候，为什么二进制会变成了字符呢？

这就是码表存在的意义。
码表其实就是一个字符和其对应的二进制相互映射的一张表。
这张表中规定了字符和二进制的映射关系。

计算机存储字符时将字符查询码表，然后存储对应的二进制。
计算机取出字符时将二进制查询码表，然后转换成对应的字符显示。

不同的码表所容纳的字符映射也是不同的。

可以这样理解。
在有些码表中一个字符占用1个字节，1个字节能表示的范围是-128到127，总共为256。所以能容纳256个字符映射。
而有些码表中一个字符占用2个字节，甚至3个字节，因此能容纳的字符映射也更多。

下面按照自己的理解详细讲述一下不同的码表。
常见的码表：
ASCII：
	* 美国码表，码表中只有英文大小写字母、数字、美式标点符号等。每个字符占用1个字节，所有字符映射的二进制都为正数，因此有128个字符映射关系。

GB2312：
	* 兼容ASCII码表，并加入了中文字符，码表中英文大小写字母、数字、美式标点符号占一个字节，中文占两个字节，中文映射的二进制都是负数，因此有128× 128 = 16384个字符映射关系。

GBK/GB18030：
	* 兼容GB2312码表，英文大小写字母、数字、美式标点符号，占一个字节。中文占两个字节，第一个字节为负数，第二个字节为正数和负数，因为有128× 256 = 32768个字符映射关系。					

Unicode码表：
	* 国际码表，包含各国大多数常用字符，每个字符都占2个字节，因此有65536个字符映射关系。Java语言使用的就是Unicode码表。
	* Java中的char类型用的就是这个码表。char c = 'a';占两个字节。

UTF-8码表：
	* 是基于Unicode码表的，但更智能，会根据字符的内容选择使用多个字节存储。英文占一个字节，中文占3个字节。

乱码的原因
	* 因为文本在存储时使用的映射码表和在读取时使用的码表不一致造成的。
```

## 3. 转换流_字符流转字节流过程

```java
OutputStreamWriter类的概述
		* 是字符流通向字节流的桥梁：字符流转字节流
		* 继承Writer，转换输出流，本质是字符流。只能往文本文件中输出数据。
		
OutputStreamWriter类构造方法
    * OutputStreamWriter(OutputStream out) 
        * 根据指定的字节输出流对象创建转换输出流对象
        * 目前可以传递的字节输出流对象有：FileOutputStream和BufferedOutputStream

    * OutputStreamWriter(OutputStream out, String charsetName) 
        * 根据指定的字节输出流对象创建转换输出流对象
        * 目前可以传递的字节输出流对象有：FileOutputStream和BufferedOutputStream
        * charsetName:指定码表的名字，可以使用码表名：GBK和UTF-8，UTF8

OutputStreamWriter类常用方法
    * write()：可以写出一个字符，一个字符数组，一个字符数组的一部分。

转换字符输出流执行过程
    * 字符转换输出流将字符查询指定码表，得到对应字节数
    * 然后将字节数交给字节输出流将字节输出到目标文件中。
```

- 示例代码

```java
public class OutputStreamWriterDemo01 {
	public static void main(String[] args) throws Exception {
		// writeGBK();
		 writeUTF8();
	}
	
	// 使用UTF8码表输出数据
	public static void writeUTF8() throws Exception {
		// 创建字符转换输出流
		OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("c.txt",true),"UTF-8");
		// 一次输出一个字符串
		osw.write("你好");
		// 关闭流
		osw.close();
	}
	// 使用GBK码表输出数据
	public static void writeGBK() throws Exception {
		// 创建字符转换输出流：默认的编码表：GBK
		OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("b.txt"));
		// 一次输出一个字符串
		osw.write("你好");
		// 关闭流
		osw.close();
	}
}
```

## 4. 转换流_字节流转字符流过程

```java
InputStreamReader类概述
	* 继承Reader，本质字符流。只能从文本文件中读取数据。
	* 是字节流通向字符流的桥梁：字节流转字符流。
	
InputStreamReader类构造方法
	* InputStreamReader(InputStream in);
	* InputStreamReader(InputStream in,String charsetName);
		* 根据指定的字节输入流对象创建字符转换输入对象
		* InputStream可以传递：FileInputStream和BufferedInputStream
		* charsetName:指定码表的名字，可以使用码表名：GBK和UTF-8，UTF8	

InputStreamReader类常用方法
	* read();读一个字符，字符数组

字符转换输入流转换过程
	* 先由指定的字节输入流从目标文件中读取字节数
	* 然后将读取到的字节交给字符输入转换流对象
	* 由字符输入转换流对象查询指定的码表得到对应的字符。
```

- 示例代码

```java
public class InputStreamReaderDemo {
	public static void main(String[] args)  throws Exception{
		readGBK();
		readUTF8();
	}
	
	/**
	 * 使用UTF8编码读取文件
	 */
	public static void readUTF8() throws Exception{
		// 创建字符串转换输入流对象
		InputStreamReader isr = new InputStreamReader(new FileInputStream("c.txt"),"UTF8");
		// 创建字符数组
		char[] buf = new char[1024];
		// 读取内容到字符数组
		int len = isr.read(buf);
		System.out.println(len);
		System.out.println(new String(buf,0,len));
		// 关闭流
		isr.close();
		
		
	}
	
	/**
	 * 使用GBK编码读取文件
	 */
	public static void readGBK() throws Exception{
		// 创建字符串转换输入流对象
		InputStreamReader isr = new InputStreamReader(new FileInputStream("c.txt"));
		// 创建字符数组
		char[] buf = new char[1024];
		// 读取内容到字符数组
		int len = isr.read(buf);
		System.out.println(len);
		System.out.println(new String(buf,0,len));
		// 关闭流
		isr.close();
	}
}
```

- 转换流转换过程图解

![](/Users/pkxing/Documents/82期就业班/day12/笔记/转换流转换过程图分析.png)

## 5. 字符流小结

- 字符流分类
  - 字符输入流
    - FileReader 继承 InputStreamReader  继承 Reader
    - BufferedReader  继承 Reader 
  - 字符输出流
    - FileWriter 继承 OutputStreamWriter  继承 Writer
    - BufferedWriter 继承 Writer
- 字符流的作用
  - 将内存中的数据保存到文本文件中
  - 将文本文件中的数据读取到内存中
- 如何选择字符流
  - 是否需要修改码表，如果需要，只能使用转换流。否则优先考虑字符缓冲流。

## 6. 对象的序列化与反序列化

- 对象的序列化的概念
  - 将对象的数据以流的形式直接保存到文件中的过程中则称为对象的序列号。
  - 实现对象的序列化需要使用到对象输出流：ObjectOutputStream
- 对象的反序列化的概念
  - 将文件中保存的对象数据以流的形式读取出来的过程则称为对象的反序列化。
  - 实现对象的的反序列化需要使用对象输入流：ObjectInputStream

## 7. ObjectOutputStream流保存对象	

- ObjectOutputStream类构造方法

     * ObjectOutputStream(OutputStream out) 
          * 可以传递的有：FileOutputStream和BufferedOutputStream

- ObjectOutputStream类常用方法

  - writeObject(Object obj); 保存对象实现对象的序列化操作。

- 注意事项

  - 要实现序列化的对象必须实现Serializable接口。
    - 对象序列化只会保存于对象相关的数据，而静态成员变量是属于类的，不属于某个对象，所以不会保存静态成员变量的值。


  - Serializable接口是一个标记性接口。

- 示例代码

```java
public class ObjectOutputStreamDemo {
	public static void main(String[] args)throws Exception {
		// 创建学生对象
		Student s = new Student("jack",23);
		s.setIdCard("9527");
		// 创建对象输出流
		ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("stu.txt"));
		// 保存对象到流关联目标文件中
		oos.writeObject(s);
		// 关闭流
		oos.close();
	}
}
```

## 8. ObjectInputStream流读取对象

- ObjectInputStream类构造方法
  - ObjectInputStream(InputStream in)
  -  根据字节输入流对象创建对象输入流
  - 可以传递：FileInputStream和BufferedInputStream
- ObjectInputStream类常用方法
  - Object readObject(); 	从流关联的目标文件中读取对象数据
- 示例代码

```java

public class ObjectInputStreamDemo {
	public static void main(String[] args) throws Exception {
		// 创建对象输入流
		ObjectInputStream ois = new ObjectInputStream(new FileInputStream("stu.txt"));
		// 读取对象数据
		Student stu = (Student)ois.readObject();
		System.out.println(stu);
		// 关闭流
		ois.close();
	}
}
```

## 9. transient和Serializable关键字

- transient关键字
  - 用来修饰成员变量的
  - 能够保证该成员变量的值在序列化对象时不被保存到文件中。
- transient格式

```java
权限修饰符 transient 数据类型 变量名;
```

- Serializable关键字
  - 标记性接口，标识该类的对象可以实现序列化操作。

## 10. 序列化中的序列号冲突问题
* 问题产生原因
	* 当一个类实现Serializable接口后，创建对象并将对象写入文件，之后更改了源代码(比如：将成员变量的修饰符有private改成public)，更改源码后并没有再次将对象写入文件，又再次从文件中读取对象时会报异常。
	
* 解决序列号冲突
	* 指定一个固定的序列号。

* 固定序列号的定义方式
	* private static final long serialVersionUID = 1478652478456L;
	* 这样每次编译类时生成的serialVersionUID值都是固定的

## 11. 打印流概述和使用

### 11.打印流概述

- 打印流的分类
  -  PrintStream:字节打印流，可以往任意类型的文件输出数据。
  -  PrintWriter:字符打印流，只能往文本文件中输出数据。


-  打印流的作用
  -  为其它流添加输出数据的功能，使其能够方便输出各种数据类型的值。
- 示例代码

```java
public class PrintStreamDemo {
	public static void main(String[] args) throws Exception {
		// 创建打印流对象：输出流
		// PrintStream ps = new PrintStream("ps.txt");
		
		PrintWriter ps = new PrintWriter("ps.txt");
		ps.println(97);
		ps.println('a');
		ps.println("你好");
		ps.println(10.5);
		
		// 关闭流
		ps.close();
	}
}
```

## 12. 第三方工具CommonsIO使用
### 12.1 使用步骤

- 在项目的根目录下创建一个文件夹：lib
- 将第三方jar包拷贝到lib文件夹下
- 选中Jar：右键—> Build Path —> add to Build Path

### 12.2 FilenameUtils工具类常用方法

```java
String getExtension(String path)
  获取文件的扩展名
String getName()
  获取文件名
boolean isExtension(String fileName,String ext)
  判断fileName是否是ext后缀名
```

- 示例代码

```java
/**
  FilenameUtils工具类常用方法
  	* static String getName(String path); 根据路径获得文件名
  	* static boolean isExtension(String filename, String extension) 
  		* 判断文件的后缀名是否是指定的名字，如果是返回true否则false
  	* static String getExtension(String filename)
  		*  获得文件的后缀名
 */
public class CommonIODemo01 {
	public static void main(String[] args) {
		String name = FilenameUtils.getName("aaa.txt");
		boolean b = FilenameUtils.isExtension("aaa.txt","png");
		System.out.println(FilenameUtils.getExtension("aaa.png"));
		System.out.println(name);
		System.out.println(b);
	}
}
```

### 12.3 FileUtils工具类常用方法

```java
readFileToString(File file)
  读取文件内容，并返回一个String
writeStringToFile(File file，String content)
  将内容content写入到file中
copyDirectoryToDirectory(File srcDir,File destDir)
  文件夹复制
copyFile(File srcFile,File destFile)
  文件复制
```

- 示例代码

```java

/**
  FileUtils工具类常用方法
  	* static String readFileToString(File file)
  		* 使用默认码表从指定的文件中读取数据  
  	* static String readFileToString(File file, String encoding) 
  		* 从指定的文件中读取数据，并指定码表。  
  	
  	* static void write(File file, CharSequence data)
  		* 将字符串输出到目标文件中，使用默认码表
  	
  	* static void copyFile(File srcFile, File destFile)
  		* 文件复制
 */
public class CommonIODemo02 {
	public static void main(String[] args) throws IOException {
		// 读取文件内容:默认gbk编码
		// String content = FileUtils.readFileToString(new File("a.txt"));
		
		/*// 读取文件内容:默认utf8编码
		String content = FileUtils.readFileToString(new File("a.txt"), "utf8");
		System.out.println(content);
		
		// 将指定的字符串输出到目标文件中
		FileUtils.write(new File("f.txt"), "你妹呀","utf8");
	
		// 文件复制
		FileUtils.copyFile(new File("aaa.png"), new File("bbb.png"));*/

		// 文件夹复制
		FileUtils.copyDirectory(new File("/Users/pkxing/documents/day01"), new File("/Users/pkxing/documents/day02"));
	}
}
```

## 13. 使用Eclipse生成jar包 


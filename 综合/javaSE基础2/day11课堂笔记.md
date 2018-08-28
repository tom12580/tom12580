# 学习目标

- 能够使用字节输出流写出数据到文件
- 能够使用字节输入流读取数据到程序
- 能够读取数据read(byte[])方法的原理
- 能够使用字节流完成文件的复制
- 能够使用字节缓冲流读取数据到程序
- 能够使用字节缓冲流写出数据到文件
	 能够完成单级文件夹复制	

#今天知识点

- IO流之字节流

## 1. IO操作概述 

- IO操作的概念
  - Input：输入操作
  - Output：输出操作
- IO操作：输入和输出操作


- IO操作的参照物：Java程序
- 输入输出操作的作用
  - 输出：将程序中数据保存到硬盘上的文件中。
  - 输入：将硬盘上的文件数据读取到内存中。

## 2. 字符流输入输出流回顾
* 将一个文本文件的内容复制到另一个文件中

## 3. 数据在计算机中的表现形式

- 计算机只能识别0和1
- 任何数据在计算机中都是0和1组成


- 所有的数据在传输过程中都需要转为二进制进行传输。


- 数据在计算机中的表现形式：二进制

```java
0b01101100001  二进制
0234356  八进制
0x10   十六进制  
34512   十进制
```

## 4. 字节输出流OutputStream

```java
OutputStream类概述
		* Output：输出流
		* Stream：字节流
		* 字节输出流：该类是所有字节输出流的父类，是一个抽象类。		
OutputStream类的常用方法
    * void close() 关闭流释放资源
    * void write(byte[] b) 将字节输出中的内容输出到流关联的目标文件中。
    * void write(byte[] b, int off, int len) 
        * 将字节数组中的一部分内容输出到目标文件中
        * off：起始索引
        * len：输出的字节个数
    * abstract void write(int b) 写一个字节到目标文件中

OutputStream类常用子类
    * FileOutpuStream
    * BufferedOutputStream
```

## 5. 字节输出流FileOutputStream写字节

- FileOutpuStream类构造方法

```java
* FileOutputStream(String name)
    * 根据指定的文件路径字符串创建文件输出流对象。
    * 如果文件不存在，则会自动创建该文件。 
* FileOutputStream(String name, boolean append) 
    * 根据指定的文件路径字符串创建文件输出流对象。
    * 如果文件不存在，则会自动创建该文件。
    * append：是否追加输出，true：是  false：不追加
```

- 示例代码

```java
// 一次输出一个字节
public static void test01() throws Exception {
    // 创建文件输出流对象
    FileOutputStream fos = new FileOutputStream("c.txt");

    // 定义整型变量
    // int a = 97; // 4个字节
    int a = 0b00000000000000000000000101100001;
    int b = 0b100101100;
    // 写出一个字节
    fos.write(b);

    // 关闭流fos
    fos.close();
}
```

## 6. 字节输出流FileOutputStream写字节数组

```java
// 一次输出一个字节数组
public static void test02() throws Exception {
    // 创建文件输出流对象
    FileOutputStream fos = new FileOutputStream("c.txt");

    // 定义一个字节数组
    byte[] buf = {97,98,99,100,101};
    // 将字节数组输出到目标文件中
    // fos.write(buf);
    fos.write(buf,1,3);

    // 定义一个字符串
    String str = "黑马程序员";
    // 将字符串转换为字节数组
    byte[] b = str.getBytes();
    fos.write(b);

    // 关闭流fos
    fos.close();
}
```

## 7. 文件的续写和换行符号

```java
// 一次输出一个字节数组：追加形式和换行输出
public static void test03() throws Exception {
    // 创建文件输出流对象
    FileOutputStream fos = new FileOutputStream("c.txt",true);

    // 定义一个字节数组
    byte[] buf = {97,98,99,100,101};
    // 将字节数组输出到目标文件中
    fos.write(buf);
    // 输出换行符
    fos.write("\r\n".getBytes());

    // 关闭流fos
    fos.close();
}
```

## 8. 字节输入流InputStream

```java
InputStream类概述
	* Input:输出流
	* Stream:字节流
	* 字节输入流，可以操作任意类型的文件。
	* 是所有字节输入流的父类。
	* 是一个抽象类，不能直接创建对象。
InputStream类成员方法
	* abstract int read(); 
		* 从流关联的目标文件中读取一个字节，返回读取到的字节。
		* 如果读取文件末尾，则返回值为-1
	* int read(byte[] b); 
		* 从流关联的目标文件中读取内容到字节数组，返回实际读取的字节个数。
	* int read(byte[] b, int off, int len) 
		* 从流关联的目标文件中读取数据到字节数组b中
		* off：起始索引
		* len：长度
	* void close(); 关闭流释放资源
InputStream类常用子类
	* FileInputStream
	* BufferedInputStream
```

## 9. 字节输入流FileInputStream读取字节

- FileInputStream类构造方法

```java
* FileInputStream(String pathname);根据文件路径字符串创建文件字节输入流对象
* FileInputStream(File file); 根据文件对象创建文件字节输入流对象。
```

- 示例代码

```java
/**
 * FileInputStream：一次读取一个字节
 * @throws Exception
 */
public static void test01() throws Exception{
    // 创建字节输入流对象
    FileInputStream fis = new FileInputStream("c.txt");
    // 读取一个字节

    /*
    int len = fis.read();
    System.out.println((char)len);

    len = fis.read();
    System.out.println((char)len);

    len = fis.read();
    System.out.println((char)len);

    len = fis.read();
    System.out.println((char)len);

    len = fis.read();
    System.out.println((char)len);

    len = fis.read();
    System.out.println(len);
    */

    // 定义变量接收实际读取字节
    // 局部变量在第一次使用之前必须完成初始化操作
    int len = -1;
    // 使用循环改进：一次读取一个字节
    while((len = fis.read()) != -1) {
        System.out.print((char)len);
    }
    // 关闭流
    fis.close();
}
```

![](一次读取一个字节.png)



## 10. 字节输入流FileInputStream读取字节数组

```java
/**
 * FileInputStream：一次读取一个字节数组
 */
public static void test02() throws Exception {
    // 创建字节输入流
    FileInputStream fis = new FileInputStream(new File("c.txt"));

    // 使用循环改进：一次读取一个字节数组
    byte[] b = new byte[1024];
    // 定义一个变量接收实际读取的字节个数
    int len = -1; 
    while((len = fis.read(b)) != -1) {
        System.out.println(new String(b,0,len));
    }

    /* 没有使用循环
    // 定义字节数组：用来存储读取到的字节
    byte[] b = new byte[5];
    // 读取内容到数组中
    int len =  fis.read(b);
    System.out.println(len); // 2
    System.out.println(new String(b)); // "abcde"

    // 再读取一次
    len = fis.read(b);
    System.out.println(len); // -1
    System.out.println(new String(b)); // cd

    // 再来读取一次
    len = fis.read(b);
    System.out.println(len); // 1   
    System.out.println(new String(b,0,len)); //   e 

    // 再来读取一次
    /*len = fis.read(b);
    System.out.println(len); // -1 
    System.out.println(new String(b)); //   e
*/		
    // 关闭路
    fis.close();
}
```

![](一次读取一个字节数组.png)

## 11. 字节流复制文件之一次读写一个字节

```Java
/**
 * 使用字节流复制文件
 */
public class FileCopy01 {
	public static void main(String[] args) {
		// 创建源文件对象
		File srcFile = new File("01-类定义复习-(掌握).itheima");
		// 创建目标文件对象
		File destFile = new File("01-类定义复习-(复制).itheima");
		
		// 调用方法复制文件：一次读写一个字节
		copyFile01(srcFile, destFile);
	}
	
	/**
	 * 文件复制：一次读写一个字节
	 */
	public static void copyFile01(File srcFile,File destFile) {
		try(		// 创建字节输入流对象：关联源文件
				FileInputStream fis = new FileInputStream(srcFile);
				// 创建字节输出流对象：关联目标文件
				FileOutputStream fos = new FileOutputStream(destFile);
				){
			// 定义变量用来接收实际读取到的字节
			int len = -1;
			// 循环读写文件
			while((len = fis.read()) != -1) {
				// 将读取到的字节输出到目标文件中
				fos.write(len);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

## 12. 字节流复制文件之一次读写一个字节数组

```java
/**
 * 使用字节流复制文件
 */
public class FileCopy01 {
	public static void main(String[] args) {
		// 创建源文件对象
		File srcFile = new File("01-类定义复习-(掌握).itheima");
		// 创建目标文件对象
		File destFile = new File("01-类定义复习-(复制).itheima");
		// 调用方法复制文件：一次读写一个字节数组
		copyFile02(srcFile, destFile);
	}
	/**
	 * 文件复制：一次读写一个字节数组
	 */
	public static void copyFile02(File srcFile,File destFile) {
		try(		// 创建字节输入流对象：关联源文件
				FileInputStream fis = new FileInputStream(srcFile);
				// 创建字节输出流对象：关联目标文件
				FileOutputStream fos = new FileOutputStream(destFile);
				){
			// 定义字节数组用来接收实际读取到的字节
			byte[] b = new byte[1024];
			// 定义变量用来接收实际读取到的字节
			int len = -1;
			// 循环读写文件
			while((len = fis.read(b)) != -1) {
				// 将读取到的字节输出到目标文件中
				fos.write(b,0,len);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

## 13. 缓冲流/高效流概述

- 高效流的高效原理

  - 使用缓冲区临时存储数据，当缓冲区满了，统一调用底层资源将数据输出到目标文件中，减少调用底层资源的次数，从而提供读写效率。

- 字节缓冲流的分类

  - 字节缓冲输入流：BufferedInputStream
  - 字节缓冲输出流：BufferedOutputStream

  ![](缓冲流的原理.png)

## 14. 字节输出缓冲流BufferedOutputStream

```java
BufferedOutputStream类概述
 		* 继承OutputStream
 		
 	BufferedOutputStream类构造方法
 		* BufferedOutputStream(OutputStream out);
 			* 根据字节输出流对象创建字节缓冲输出流对象。
 			* 目前可以传递的字节输出流：FileOutputStream
 			* 传递谁就提供谁的效率
 		
 	BufferedOutputStream类成员方法	
 		* void flush(): 将缓冲区的数据直接输出到目标文件中
 		* void close() 关闭流释放资源
		* void write(byte[] b) 将字节输出中的内容输出到流关联的目标文件中。
		* void write(byte[] b, int off, int len) 
			* 将字节数组中的一部分内容输出到目标文件中
			* off：起始索引
			* len：输出的字节个数
		* void write(int b) 写一个字节到目标文件中
 
 	注意事项
 		* 使用字节输出缓冲流输出数据不是直接输出到目标文件中，而是先输出到缓冲区数组中
 		* 只有当缓冲区数组满了或调用flush/close方法时，才会将缓冲区数组中的内容输出到目标文件中。
```

- 示例代码

```java
public class BufferedOutputStreamDemo01 {
	public static void main(String[] args) throws Exception{
		// 创建文件字节输出流对象
		// FileOutputStream bos = new FileOutputStream("e.txt");
		
		// 创建字节缓冲输出流对象
		BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("e.txt"));
		
		// 输出数据
		// bos.write("abcdef".getBytes());
		bos.write(97);
		
		// 刷新缓冲区：
		// bos.flush();
		
		// 关闭流
		 bos.close();
	}
}
```

## 15. 字节输入缓冲流BufferedInputStream		  

```java
BufferedInputStream类概述
 		* 继承InputStream
 		
BufferedInputStream类构造方法
    * BufferedInputStream(InputStream out);
        * 根据字节输入流对象创建字节缓冲输入流对象。
        * 目前可以传递的字节输入流：FileInputStream
        * 传递谁就提供谁的效率

BufferedInputStream类成员方法	
    * read();可以读取一个字节，一个字节数组
 
注意事项
    * 不是直接从目标文件中读取数据，而是从缓冲区数组中读取，
    * 如果缓冲区数组没有数据，则会使用字节输入流对象从目标文件中一次读取8192个字节到缓冲区数组中。
```

- 示例代码

```java
public class BufferedInputStreamDemo02 {
	public static void main(String[] args) throws Exception{
		// 创建字节缓冲输入流对象
		BufferedInputStream bis = new BufferedInputStream(new FileInputStream("e.txt"));
		
		// 读取一个字节
		/*
		int len = -1;
		while((len = bis.read()) != -1) {
			System.out.print((char)len);
		}*/
		
		// 读取一个字节数组
		// 定义变量用来接收实际读取到字节个数
		int len = -1;
		// 创建字节数组
		byte[] buf = new byte[1024];
		// 循环读取
		while((len = bis.read(buf)) != -1) {
			System.out.println(new String(buf,0,len));
		}
		// 关闭流
		bis.close();
	}
}
```

## 16. 四种文件复制方式的效率比较

```java

/**
 * 使用字节流复制文件
 */
public class FileCopy01 {
	public static void main(String[] args) {
		// 创建源文件对象
		File srcFile = new File("aaaa.itheima");
		// 创建目标文件对象
		File destFile = new File("01-类定义复习-(复制).itheima");
		
		// 调用方法复制文件：一次读写一个字节
		// copyFile01(srcFile, destFile);
		// 调用方法复制文件：一次读写一个字节数组
		copyFile02(srcFile, new File("02-类定义复习-(复制).itheima"));
		copyFile03(srcFile, new File("03-类定义复习-(复制).itheima"));
		copyFile04(srcFile, new File("04-类定义复习-(复制).itheima"));
	}
	
	/**
	 * 文件复制：一次读写一个字节
	 */
	public static void copyFile01(File srcFile,File destFile) {
		try(		// 创建字节输入流对象：关联源文件
				FileInputStream fis = new FileInputStream(srcFile);
				// 创建字节输出流对象：关联目标文件
				FileOutputStream fos = new FileOutputStream(destFile);
				){
			// 定义变量用来接收实际读取到的字节
			int len = -1;
			// 循环读写文件
			while((len = fis.read()) != -1) {
				// 将读取到的字节输出到目标文件中
				fos.write(len);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 非缓冲流文件复制：一次读写一个字节数组
	 */
	public static void copyFile02(File srcFile,File destFile) {
		long start = System.currentTimeMillis();
		try(		// 创建字节输入流对象：关联源文件
				FileInputStream fis = new FileInputStream(srcFile);
				// 创建字节输出流对象：关联目标文件
				FileOutputStream fos = new FileOutputStream(destFile);
				){
			// 定义字节数组用来接收实际读取到的字节
			byte[] b = new byte[1024];
			// 定义变量用来接收实际读取到的字节个数
			int len = -1;
			// 循环读写文件
			while((len = fis.read(b)) != -1) {
				// 将读取到的字节输出到目标文件中
				fos.write(b,0,len);
			}
			System.out.println("非缓冲流文件复制：一次读写一个字节数组 = " + (System.currentTimeMillis() - start));
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 缓冲流文件复制：一次读写一个字节数组
	 */
	public static void copyFile03(File srcFile,File destFile) {
		long start = System.currentTimeMillis();
		try(		// 创建字节输入流对象：关联源文件
				BufferedInputStream fis = new BufferedInputStream(new FileInputStream(srcFile));
				// 创建字节输出流对象：关联目标文件
				BufferedOutputStream fos = new BufferedOutputStream(new FileOutputStream(destFile));
				){
			// 定义字节数组用来接收实际读取到的字节
			byte[] b = new byte[1024];
			// 定义变量用来接收实际读取到的字节个数
			int len = -1;
			// 循环读写文件
			while((len = fis.read(b)) != -1) {
				// 将读取到的字节输出到目标文件中
				fos.write(b,0,len);
			}
			System.out.println("缓冲流文件复制：一次读写一个字节数组 = " + (System.currentTimeMillis() - start));
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 缓冲流文件复制：一次读写一个字节
	 */
	public static void copyFile04(File srcFile,File destFile) {
		long start = System.currentTimeMillis();
		try(		// 创建字节输入流对象：关联源文件
				BufferedInputStream fis = new BufferedInputStream(new FileInputStream(srcFile));
				// 创建字节输出流对象：关联目标文件
				BufferedOutputStream fos = new BufferedOutputStream(new FileOutputStream(destFile));
				){
			// 定义变量用来接收实际读取到的字节
			int len = -1;
			// 循环读写文件
			while((len = fis.read()) != -1) {
				// 将读取到的字节输出到目标文件中
				fos.write(len);
			}
			System.out.println("缓冲流文件复制：一次读写一个字节 = " + (System.currentTimeMillis() - start));
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
}
```

## 17. `flush`方法和`close`方法区别

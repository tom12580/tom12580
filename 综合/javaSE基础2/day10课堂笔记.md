# 知识点回顾

- Map集合

  - 双列集合
  - 键必须唯一
  - 值可以重复

- Map接口常用实现类

  - HashMap
  - LinkedHashMap：继承HashMap，能够保证存储顺序一致。

- Map接口常用方法

  ```java
  V put(K key,V value)
  	添加和修改键值对
  	有则修改，无则添加。
  V get(Object key); 根据键获得值
  V remove(Object key); 根据键删除键值对。
  int size(); 获得键值对的个数
  Set<K> keySet();获得键的集合
  Set<Entry<K,V>> entrySet(); 获得Entry对象集合
  ```

- Entry对象的常用方法

  ```java
  每一个键值对都对应一个Entry对象
  K getKey(); 获得键
  V getValue()；获得值
  ```

- 可变参数

  - 参数个数任意，可以0到n个
  - 格式：数据类型…参数名
  - 本质：数组
  - 注意事项：一个参数列表中只能有一个可变参数并只能是参数列表的最后一个参数。

- Arrays工具类常用方法

```java
static String toString(int[] arr); 将数组的元素拼接成字符串
static void sort(int[] arr); 对数组元素进行排序，默认是升序
static int binarySearch(int[] arr,int key); 使用二分法查找指定元素在数组中的索引值，如果找到，返回对应的索引值，如果找不到返回值=-插入点-1；数组必须是有序的，
static boolean equals(int[] arr1,int[] arr2); 比较两个数组中的元素是否完全相同。
static void fill(int[] arr,int value);使用指定的值填充数组的元素。
static List<T> asList(T...a); 数组转集合，集合长度固定，不能增删元素。
```

- Collections工具类常用方法

```java
static void sort(List<T> list)；对集合元素进行排序，默认是升序
static void shuffle(List<T> list); 对集合的元素乱序
static int binarySearch(List<T> list, T key); 使用二分法查找指定元素在集合中的索引值，如果找到，返回对应的索引值，如果找不到返回值=-插入点-1；集合必须是有序的，
static void addAll(Collection c, T...t); 将数组中的元素添加指定的集合中。
```

- ArrayList类中集合转数组的方法

```java
Object[] toArray(); 将集合转为对象数组
T[] toArray(T[] arr); 将集合中的元素存储到指定的数组中。
```



# 学习目标

- 能够说出File对象的创建方式
- 能够说出File类获取名称的方法名称
- 能够说出File类获取绝对路径的方法名称
- 能够说出File类获取文件大小的方法名称
- 能够说出File类判断是否是文件的方法名称
- 能够说出File类判断是否是文件夹的方法名称
- 能够辨别相对路径和绝对路径
- 能够遍历文件夹
- 能够解释递归的含义
- 能够使用递归的方式计算5的阶乘
- 能够说出File类获取绝对路径的方法名称				
- 能够说出使用递归会内存溢出隐患的原因

# 今日内容

- File类
- 递归概念

## 1. File类的概述和作用

- 需求：硬盘上有一个文件：a.txt，如何获得文件的大小？


- File类的概述
  -  文件和文件夹的抽象表示形式。
  -  一个File类对象就对应一个文件或文件夹。 	


-  File类的作用
  -  用来操作硬盘上的文件或文件夹。

## 2. File类静态的成员变量

```java
* static String	pathSeparator 
 		* 与系统有关的路径分隔符
 		* 不同的操作系统，该分隔符是不一致的
 		* mac系统是  :
 		* window系统是 ;
* static String	separator
 		* 与系统有关的目录分隔符
 		* mac系统是  /
 		* window系统是 \
```

- 示例代码

```java
public class FileDemo01 {
	public static void main(String[] args) throws Exception{
		System.out.println(File.pathSeparator);
		System.out.println(File.separator);
		
		// 创建字符输入流
		FileReader fr = new FileReader("aaa"+File.separator+"aaa.txt");
		System.out.println(fr.read());
		// 关闭流
		fr.close();
	}
}
```

## 3. 绝对路径和相对路径

- 绝对路径
  - 以盘符开始：`c://java/a.txt`
  - 在系统中具有唯一性
- 相对路径
  - 相对某一个位置而言：`c://xxx/yyy  a.txt`
  - 在系统中不具有唯一性
    - `c://xxx/yyy  a.txt`
    - `d://xxx//yyy  a.txt`

## 4. File类构造方法

```java
* File(String pathname) 
    * 根据文件路径字符串创建文件对象
* File(String parent, String child) 
    * 根据父路径字符串和子路径字符创建文件对象。
* File(File parent, String child) 
    * 根据父路径文件对象和子路径字符创建文件对象。
```

- 示例代码

```java
public class FileDemo02 {
	public static void main(String[] args) throws Exception{
		 // 创建文件对象: 使用相对路径，相对当前项目的根目录
		File f1 = new File("a.txt");
		// 创建文件对象: 使用绝对路径
		File f2 = new File("/Users/pkxing/Documents/aaa.png");
		
		System.out.println(f1);
		System.out.println(f2);
		
		File f3 = new File("aaa","/b.txt");
		File f4 = new File(new File("aaa"),"/b.txt");
		File f5 = new File("aaa/b.txt");
		System.out.println(f3);
		System.out.println(f4);
		System.out.println(f5);
	}
}
```

## 5. File类创建文件功能

```java
boolean createNewFile();
    * 根据文件对象指定的文件路径创建文件
    * 如果创建成功，返回true，否则返回false
    * 如果文件存在，则不创建，返回false。
    * 注意：该方法只能用来创建文件，不能用来创建文件夹。
```

- 示例代码

```java
public class FileDemo03 {
	public static void main(String[] args) throws Exception{
		 // 创建文件对象
		File f = new File("/Users/pkxing/documents/bbb");
		// 创建文件
		boolean b = f.createNewFile();
		System.out.println(b);
	}
}
```

## 6. File类创建文件夹功能

```java
* boolean mkdir();  
    * 创建文件夹，创建成功返回true，否则false
    * 只能用来创建文件夹,只能创建单级文件夹
    * (mk ==> make)
    * (dir ==> directory)

* boolean mkdirs();
    * 创建文件夹，可以创建单级或多级文件夹。 
    * 创建成功返回true，否则返回false
```

- 示例代码

```java
public class FileDemo04 {
	public static void main(String[] args) throws Exception{
		 // 创建文件对象
		File f = new File("/Users/pkxing/documents/bbb.txt");
		// 创建文件夹:创建单级文件夹
		// boolean b = f.mkdir();
		// 创建文件夹:创建多级文件夹
		boolean b = f.mkdirs();
		System.out.println("b = " + b); 
	}
}
```

## 7. File类删除功能

```java
boolean delete()
	* 删除文件对象关联路径下文件
	* 删除成功返回true，否则返回false 
	* 注意：如果是普通文件，则可以直接删除，如果是文件夹，则只能删除空文件夹。
```

- 示例代码

```java
public class FileDemo05 {
	public static void main(String[] args) throws Exception{
		 // 创建文件对象
		File f = new File("/Users/pkxing/Documents/bbb");
		// 删除文件
		boolean b = f.delete();
		System.out.println(b);
	}
}
```

## 8. File类获取功能	

```java
File文件获取功能的方法
* public long length()  
    * 获得文件的大小：字节 
    * 如果关联的是文件夹路径，则返回值是一个不确定的值。

* public String getName() 
    * 获得文件名或文件夹名

* String	 getParent() 
    * 获得父路径字符串,返回字符串

* File getParentFile();
    * 获得父路径字符串,返回文件对象

* String getAbsolutePath()
    * 获得文件的绝对路径字符串,返回字符串

* File getAbsoluteFile();
    * 获得文件的绝对路径字符串,返回文件对象
```

- 示例代码

```java
public class FileDemo06 {
	public static void main(String[] args) throws Exception{
		// 创建文件对象
		File f = new File("/Users/pkxing/Documents/aaa.png");
		// 获得文件的大小：字节 
		long length = f.length();
		System.out.println(length);
		
		// 获得文件名
		System.out.println(f.getName());
		// 获得父路径字符串,返回字符串
		System.out.println(f.getParent());
		// 获得父路径字符串,返回文件对象
		System.out.println(f.getParentFile());
		
		// 获得文件的绝对路径字符串,返回字符串
		System.out.println(f.getAbsolutePath());
		// 获得文件的绝对路径字符串,返回文件对象
		System.out.println(f.getAbsoluteFile());
	}
}
```

## 9. File类判断功能

```java
File类的判断功能的方法
 	* boolean exists(); 判断文件或文件夹是否存在，如果存在，则返回true，否则返回false。
 	* boolean isFile(); 判断文件对象封装的路径是否关联的是一个文件，如果是返回true，否则false
 	* boolean isDirectory(); 判断是否是文件夹，如果是返回true，否则返回false
```

- 示例代码

```java
public class FileDemo07 {
	public static void main(String[] args) throws Exception{
		// 创建文件对象
		File f = new File("bbb");
		// 判断是否是文件
		System.out.println(f.isFile());
		System.out.println(f.isDirectory());
		System.out.println(f.exists());
	}
}
```

## 10. File类list获取功能

```java
File[] listFiles();
  - 获得文件夹下的所有文件(子文件和子文件夹)
  - 如果指定的路径是文件，则返回的数组：null 
```

- 示例代码

```java
/**
File类list获取功能
    * File[] listFiles();
        * 获得文件夹下的所有文件(子文件和子文件夹)
        * 如果指定的路径是文件，则返回的数组：null 
 */
public class FileDemo08 {
	public static void main(String[] args) throws Exception{
		// 创建文件夹对象
		File dir = new File("/Users/pkxing/Documents/aaa.itheima");
		// 获得该文件下的所有文件
		File[] files = dir.listFiles();
		if(files != null) {
			// 遍历数组
			for (File file : files) {
				if(file.isDirectory()) {
					System.out.println(file);				
				}
			}
		}
	}
}
```

## 11. 递归概述

```java
递归的概念
	* 指方法内部自己调用自己的过程则称为递归。
递归的分类
	* 直接递归：方法调用自身
	* 间接递归：A方法调用B方法，B方法调用C方法，C方法调用A方法
递归的注意事项
	* 递归必须要有出口：结束递归的条件
	* 递归的次数不要太多。
	* 构造方法不能使用递归。
```

## 12. 递归练习

### 12.1 求和计算

* 需求说明
  * 求1到n的和。
* 代码实现

```java
/**
 * 求1到n的和
 	 当n=5时
 	 sum(5) = 5 + sum(4)
 	 sum(4) = 4 + sum(3)
 	 sum(3) = 3 + sum(2)
 	 sum(2) = 2 + sum(1)
 	 sum(1) = 1
 	 
 	 sum(n) = n + sum(n-1)
 	 
 */
public class DGDemo03 {
	public static void main(String[] args) {
		System.out.println(sum(100));
	}
	
	/**
	 * 求1到n的和
	 * @param n
	 * @return 求和结果
	 */
	public static int sum(int n) {
		if(n == 1) return 1;
		// return 3;
		return n + sum(n - 1);
	}
}
```

### 12.2 递归求阶乘
*  需求说明
  * 求n的阶乘。
*  代码实现

```java
/**
 * 求n的阶乘
 	jc(3) = 3 * jc(2)
 	jc(2) = 2 * jc(1)
 	jc(1) = 1
 	
 	jc(n) = n * jc(n-1);
 */
public class DGDemo04 {
	public static void main(String[] args) {
		System.out.println(jc(1));
		System.out.println(jc(5));
		System.out.println(jc(10));
	}
	
	/**
	 * 求n的阶乘
	 * @param n
	 * @return 求和结果
	 */
	public static int jc(int n) {
		if(n == 1) return 1;
		return n * jc(n-1);
	}
}
```

### 12.3 递归实现斐波那契数列

- 需求说明
  - 有一对兔子，从出生后第3个月起每个月都生一对兔子，小兔子长到第三个月后每个月又生一对兔子，假如兔子都不死，问第二十个月兔子对数为多少？
- 代码实现

```java
/**
需求说明
- 有一对兔子，从出生后第3个月起每个月都生一对兔子，小兔子长到第三个月后每个月又生一对兔子，
- 假如兔子都不死，问第二十个月兔子对数为多少？
	1 1
	2 1
	3 2
	4 3
	5 5
	6 8
	...
	
	sum(1) = 1;
	sum(2) = 1
	sum(3) = sum(1) + sum(2);
	sum(4) = sum(3) + sum(2);
	.......
	sum(20) = sum(n-1) + sum(n-2);
 */
public class DGDemo05 {
	public static void main(String[] args) {
		System.out.println(sum(1));
		System.out.println(sum(2));
		System.out.println(sum(6));
	}
	
	public static int sum(int month) {
		if(month == 1 || month == 2) return 1;
		return sum(month - 1) + sum(month-2);
	}
}
```

### 12.4. 递归遍历多级文件夹下文件名

- 需求说明
  - 遍历文件夹，将文件夹中所有的文件名输出，如果文件夹里面还有文件夹，也需要将这个文件里面的所有文件名输出。
- 示例代码

```java

/**
 需求：
	遍历文件夹，将文件夹中所有的文件名输出，如果文件夹里面还有文件夹，
	也需要将这个文件里面的所有文件名输出。
 */
public class DGDemo06 {
	public static void main(String[] args) {
		// 创建文件夹对象
		File dir = new File("/Users/pkxing/Documents/82期就业班");
		printFileName(dir);
	}
	
	/**
	 * 将指定文件夹dir下的所有的文件名打印输出(包括子文件下的)
	 * @param dir
	 */
	public static void printFileName(File dir) {
		// 判断是否存在
		if(!dir.exists())return;
		// 判断是否是文件夹
		if(!dir.isDirectory())return;
		
		// 获得dir文件夹下的所有的文件
		File[] files = dir.listFiles();
		// 遍历数组文件
		for (File file : files) {
			// 判断是否是普通文件
			if(file.isFile()) {
				// 输出文件名
				System.out.println(file.getName());
			} else {
				// 文件夹
				printFileName(file);
			}
		}
	}
	
}
```

## 13. 文件过滤器
* 需求说明
	* 键盘输入一个文件夹路径。
	* 获得该文件夹以及子文件夹下的所有Java文件并将文件名输出到控制台。


```java

/**
 * 自定义文件过滤器
 * @author pkxing
 *
 */
public class MyFileFilter implements FileFilter {

	/**
	 	该方法的调用时机？
			* 每当获得到一个文件或文件夹时，都会将该文件或文件夹封装成文件对象，然后调用过滤器
			对象的该方法。
			
		该方法的作用？
			* 用来过滤文件，返回true代表该文件不过滤，返回false代表过滤。
	 */
	@Override
	public boolean accept(File pathname) {
		// System.out.println("-------"+pathname);
		// 判断是否是itheima结尾的文件
		if(pathname.isDirectory() || pathname.getName().endsWith(".itheima")) {
			// 返回true表示不过滤
			return true;
		}
		// 返回false，表示不接受该文件，过滤掉
		return false;
	}
}

public class DGDemo08 {
	public static void main(String[] args) {
		// 创建文件夹对象
		File dir = new File("/Users/pkxing/Documents/82期就业班");
		printFileName(dir);
	}
	
	/**
	 * 将指定文件夹dir下的所有的文件名打印输出(包括子文件下的)
	 * @param dir
	 */
	public static void printFileName(File dir) {
		// 判断是否存在
		if(!dir.exists())return;
		// 判断是否是文件夹
		if(!dir.isDirectory())return;
		// 获得dir文件夹下的所有的文件：使用自定义过滤器
		// File[] files = dir.listFiles(new MyFileFilter());
		
		// 使用匿名内部类实现文件过滤器
		File[] files = dir.listFiles(new FileFilter() {
			@Override
			public boolean accept(File pathname) {
				// 判断是否是itheima结尾的文件
				if(pathname.isDirectory() || pathname.getName().endsWith(".itheima")) {
					// 返回true表示不过滤
					return true;
				}
				// 返回false，表示不接受该文件，过滤掉
				return false;
			}
		});
		// System.out.println(files.length);
		// 遍历数组文件
		for (File file : files) {
			// 判断是否是普通文件
			if(file.isFile()) {
				// 输出文件名
				System.out.println(file.getName());
			} else {
				// 文件夹
				printFileName(file);
			}
		}
	}
}
```


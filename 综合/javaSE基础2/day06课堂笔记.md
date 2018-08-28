# 知识点回顾

- Object类的两个方法

```java
boolean equals(Object obj); 比较两个对象是否相同，默认通过比较地址值判断。一般子类都会重写该方法，目的是想通过比较成员变量的值来判断两个对象是否相同。

String toString();
	默认返回值：类全名@对象地址值;
	打印对象都控制台时会触发该方法调用。
	重写该方法：打印输出对象时查看对象中所有成员变量及其值是什么。
```

- 异常概述

  - 程序在运行或编译过程中出现的问题。

- 异常的处理方式

  - JVM处理方式

    - 将异常信息(异常原因，位置，类全名)打印在控制台上并结束程序的运行。

  - 手动处理方式

    - 捕获处理：

      ```Java
      try {
          // 可能出现异常的代码
      }catch(异常类名 变量名){
          // 处理异常的代码
      } ...
      catch(异常类名 变量名){
          
      } finally{
          // 一定会执行的代码：释放资源
      }
      ```

    - 抛出处理

      ```java
      throw：将异常对象抛给方法调用者，并结束当前方法的运行。
      throws：声明该方法可能会出现的异常。

      throw：使用方法体中，使用格式：throw 异常对象;
      throws:使用方法声明上，使用格式：throws 异常类名,....{}
      ```

- 运行时异常和编译时异常概述

# 学习目标

* 能够使用日期类输出当前的日期
* 能够说出将日期格式化成字符串的方法
* 说出将字符串转换成日期的方法
* 写出基本数据类型对应的八种包装类
* 能够说出拆箱装箱概念
* 能够System类常见方法
* 能够使用Math类进行数学运算
* 能够正则表达式验证11位手机号码
* 能够说出字符串的split方法作用	

# 今天内容

- 常用的API
  - Date，DateFormat，Calander，System，Math
- 包装类
- 正则表达式

## 1. Date类概述

```java
	Date类概述
 		* 该类用来描述时间日期。
 	
 	Date类常用构造方法
 		Date(); 获得当前系统的日期对象
 		Date(long date); 根据指定的毫秒值创建日期对象
 		
 	Date类常用成员方法
 		long	 getTime(); 获得当前时间的毫秒值
 		
 	毫秒值的概念
 		1秒 = 1000毫秒
 	
 	时间零的概念
 		1970.1.1 00:00:00 
 		
 		2018.4.3 8:54:55
 		1522716736621
```

测试代码

```java
public class DateDemo01 {
	public static void main(String[] args) {
		// 创建日期对象
		Date d = new Date();
		// Tue Apr 03 08:49:51 CST 2018
		System.out.println(d);
		
		// 1522716732418
		// 1522716736621
		System.out.println(d.getTime());
		
		// 创建日期对象
		Date d1 = new Date(1000); //  Thu Jan  01 00:00:01 CST 1970
		System.out.println(d1);
	}
}
```

## 2. 日期对象转换字符串

```java
DateFormat类概述
 		* 是一个抽象类，不能直接创建该类对象。要使用它的子类：SimpleDateFormat
 	
DateFormat类静态方法
 		* static DateFormat	getDateInstance() 获得日期格式化对象,返回的就是SimpleDateFormat的对象
   
SimpleDateFormat类常用操作
 		* 将日期对象格式为字符串
 		* 将日期字符串格式化为日期对象
SimpleDateFormat类构造方法
 		* SimpleDateFormat(); 使用默认的格式创建日期格式化对象
 		* SimpleDateFormat(String pattern); 使用指定的日期模式创建日期格式化对象
SimpleDateFormat类常用成员方法
 		* void applyPattern(String pattern)  修改日期模式
 		* String format(Date d) 将日期对象格式化为字符串
 		* Date parse(String str) 将字符串格式化为日期对象，字符串的格式要和日期模式一致。
如何指定日期模式？
 		年：yyyy  2018
 		月：MM    04
 		日：dd    03
 		时：HH  24小时制 hh 12小时制
 		分：mm
 		秒：ss
```

- 示例代码 

```java
public class DateFormatDemo01 {
	public static void main(String[] args) {
		// 创建日期对象
		Date d = new Date();
		System.out.println(d.toString()); // Tue Apr 03 09:06:57 CST 2018
		
		// 2018-04-03 08:49:51
		// 2018/04/03 08:49:51
		// 2018年04月03日 08:49:51
		
		// 创建日期格式化对象
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
		// 将日期对象格式化为字符串
		String dateStr = sdf.format(d);
		System.out.println(dateStr);
	}
}
```

## 3. 字符串转成日期对象

```java
public class DateFormatDemo01 {
	public static void main(String[] args) throws ParseException {
		// 字符串
		// String dateStr = "2018/04/03 08:49:51";
		String dateStr = "08:49:51";
		// 字符串 ==> 日期对象
		// 创建日期格式化对象
		// SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");
		SimpleDateFormat sdf = new SimpleDateFormat("HH:mm:ss");
		// 将字符串格式化成日期对象
		Date d = sdf.parse(dateStr);
		System.out.println(d);
	}
}
```

## 4. Calendar类的使用

```Java
Calendar类概述
 		* 是一个日历类，通过该类提供的方法可以获得时间日期信息。
 		* 也是一个抽象类，不能直接创建该类对象。		
如何创建日历对象？
 		* 通过Calendar类提供的静态方法创建，方法声明如下：
 			* static Calendar getInstance(); 创建日历对象。	
Calendar类常用方法
 		* int get(int field); 根据日历字段获得对应的值
 		* Date getTime(); 获得日期对象
 		* long getTimeInMillis(); 获得毫秒值
 		* void add(int field, int amount) 
 			* 将指定日历字段的值在当前的基础上加上一个偏移值
 			* 正数，向后偏移
 			* 负数，向前偏移
 		* void set(int field, int amount) 	
 			* 将指定日历字段的值修改为指定的值amount 	
 		* void set(int year, int month, int date) 
 			* 将日期时间设置到指定的年月日上		
注意事项
 	获得的月份需要加一才是我们当前的月份，因为月份默认是：0到11
```

示例代码

```java
public class CalenderDemo01 {
	public static void main(String[] args) {
		test01();
		test02();
	}
	/**
	 * Calendar类常用方法02
	 */
	public static void test02() {
		// 创建日历对象
		Calendar c = Calendar.getInstance();
		// 修改年月日
		c.set(1999, 11, 32);
		// c.add(Calendar.YEAR, -1);
		c.add(Calendar.MONTH, 10); // 3
		// 获得年份
		int year = c.get(Calendar.YEAR);
		int month = c.get(Calendar.MONTH);
		System.out.println(year); //  2000 
		System.out.println(month); // 9
		// set方法
		c.set(Calendar.YEAR, 2020);
		year = c.get(Calendar.YEAR);
		System.out.println(year);
	}
	
	/**
	 * Calendar类常用方法01
	 */
	public static void test01() {
		// 创建日历对象
		Calendar c = Calendar.getInstance();
		// 获得日期对象
		Date d = c.getTime();
		System.out.println(d.getTime());
		System.out.println(c.getTimeInMillis());
		// 获得年份
		int year = c.get(Calendar.YEAR);
		// 获得月份
		int month = c.get(Calendar.MONTH);
		// 获得日
		int day = c.get(Calendar.DAY_OF_MONTH);
		// 获得时分秒
		int hour = c.get(Calendar.HOUR);
		int minute = c.get(Calendar.MINUTE);
		int second = c.get(Calendar.SECOND);
		System.out.println(year+"-"+(month+1) + "-"+day);
		System.out.println(hour+":"+ minute + ":"+second);		
	}
}
```

## 5. Calendar练习

* 求出今天距离2020年1月1日还有多少天

```java
/**
 求出今天距离2020年1月1日还有多少天
 	思路：
 		获得今天时间的毫秒值
 		将时间定为到2020年1月1日
 		再获得毫秒值
 		两个毫秒值相减，得到毫秒值
 */
public class CalendarDemo02 {
	public static void main(String[] args) {
		// 获得日历对象
		Calendar c = Calendar.getInstance();
		// 获得今天时间的毫秒值
		long todayTime = c.getTimeInMillis();
		// 将时间定为到2020年1月1日
		c.set(2020, 0, 1);
		// 再获得毫秒值
		long futureTime = c.getTimeInMillis();
		// 两个毫秒值相减，得到毫秒值
		long distance = futureTime - todayTime;
		// 计算天数
		System.out.println(distance/1000/3600/24);
	}
}
```

## 6. Math类概述
### 6.1 常用方法

```java
static double sqrt(double d)：
static double pow(double a, double b)：
static double floor(double d)：
static double ceil(double d)：
static int abs(int i)：
Math.random()：
Math.round(double d)：
```

### 6.2 演示代码

```java
/**
 	Math类概述
 		* 该类提供一些数学运算相关的方法。
 	Math类常用方法
		static double sqrt(double d)：返回参数d的平方根
		static double pow(double a, double b)：返回a的b次方的值
		static double floor(double d)：返回小于或等于参数d的最大整数。
		static double ceil(double d)：返回大于或等于参数d的最小整数。
		static int abs(int i)： 返回参数i的绝对值
		Math.random()： 返回一个随机数，取值范围：[0,1) 本质还是使用Random类实现。
		Math.round(double d)：返回参数d四舍五入的结果。
		
 */
public class MathDemo {
	public static void main(String[] args) {
		System.out.println(Math.sqrt(9));
		System.out.println(Math.sqrt(4));
		System.out.println(Math.pow(2, 6));
		
		System.out.println(Math.floor(2.0));
		System.out.println(Math.floor(2.1));
		System.out.println(Math.floor(2.5));
		System.out.println(Math.floor(2.9));
		
		System.out.println(Math.ceil(2.0));
		System.out.println(Math.ceil(2.1));
		System.out.println(Math.ceil(2.5));
		System.out.println(Math.ceil(2.9));
		
		System.out.println(Math.abs(-1));
		System.out.println(Math.abs(1));
		
		System.out.println(Math.random());
		System.out.println(Math.round(4.4)); // 4
		System.out.println(Math.round(4.5)); // 5
		
	}
}
```

## 7. System类概述

### 7.1 常用方法

```java
static long currentTimeMillis(); 获得当前时间毫秒值
static void exit(); 退出JVM，终止程序运行。
static void gc(); 通知垃圾回收器进行垃圾回收。不一定会真正进行回收。
static Properties getProperties(); 获得操作系统的属性
static void arraycopy(Object src, int srcPos,Object dest, int destPos, int length);
 	* 数组复制
 	* src：源数组
 	* srcPos：源数组的起始索引
 	* dest：目标数组
 	* destPos：目标数组的起始索引
 	* length：复制的个数
```

### 7.2 演示代码

```java
public class SystemDemo {
	
	public static void main(String[] args) {
		test01();
		test02();
		test03();
		test04();
	}
	
	/**
	 * 数组拷贝方法
	 * @param args
	 */
	public static void test04() {
		int[] src = {1,2,3,4,5,6};
	 	int[] dest = new int[6];
	 	for (int i = 0; i < dest.length; i++) {
			System.out.print(dest[i]+",");
		}
	 	System.out.println();
	 	// 进行数组复制
	 	// System.arraycopy(src,0,dest,0,6);
	 	// 复制前三个元素
	 	System.arraycopy(src,0,dest,3,3);
	 	for (int i = 0; i < dest.length; i++) {
			System.out.print(dest[i]+",");
		}
	}
	
	/**
	 * static Properties getProperties(); 获得操作系统的属性
	 */
	public static void test03() {
		// 获得操作系统的属性：操作系统名称
		Properties p = System.getProperties();
		System.out.println(p.getProperty("os.name"));
	}
	
	/**
	 * static void gc(); 通知垃圾回收器进行垃圾回收。不一定会真正进行回收。
	 */
	public static void test02() {
		for (int i = 0; i < 100; i++) {
			// 创建Person对象
			new Person();
			// 通知垃圾回收器进行垃圾回收
			System.gc();
		}
	}
	
	/**
	 * static long currentTimeMillis(); 获得当前时间毫秒值
	 * @param args
	 */
	public static void test01() {
		// 获得当前时间毫秒
		long start = System.currentTimeMillis();
		System.out.println(start);
		for (int i = 0; i < 10; i++) {
			// System.out.println(i);
		}
		System.out.println(System.currentTimeMillis() - start);
	}
}
```

## 8. 基本数据类型包装类
* 场景
	* 通过文本框获得用户输入的数字数据，因为文本框里面是书写文本数据的，所以后台得到的都是字符串。如果想要对字符串中的数字进行运算，必须将字符串转换成数字。

* 怎么解决上述出现的问题？
	* Java为每一种基本类型提供了类，这些类称为基本数据类型包装类。
	* 每种包装类都提供相关的方法操作基本类型数据。 

* 包装类常见操作有
	* 将字符串转换为对应的基本数据类型。 
	* 将基本数据类型转换为字符串类型。
	
* 八种基本类型对应的包装类

  ```java
  char  	Character
  int 	Integer
  byte 	Byte
  short 	Short
  long 	Long
  float 	Float
  double 	Double
  boolean Boolean
  ```

## 9. Integer包装类的概述
* 构造方法
  * Integer(int value);将一个整数value转换成整型包装类对象。
  * Integer(String s);将一个字符串数值转换成整型包装类对象。
* 成员方法
  * int intValue();将构造方法中的字符串转换成基本类型。 
  * static int parseInt();把字符串转换为int值。
  * static String toString(int i);将整数i转换成字符串
 * 静态成员变量
     * Integer.MAX_VALUE：整数最大值
     * Integer.MIN_VALUE：整数最小值 
* 演示代码

```java
/**
  Integer包装类的概述
	- 将字符串转换为对应的基本数据类型。 "100" ==> 100 
	- 将基本数据类型转换为字符串类型。   100 ==> "100"

  Integer类构造方法
  	Integer(int value) 将基本数据类转换成包装类对象。
    Integer(String s) 根据数值字符串创建整型包装类对象，字符串一定要是是数字组成的。
   
  Integer类静态方法
    static Integer valueOf(int i)  将基本数据类型转换为包装类型
    static int parseInt(String s) 将字符串数字转换整型
    static String toString(int i) 将整型数据转换字符串
    
  Integer类成员方法
  	int intValue(); 将构造方法指定的字符串数字转换整型数字。
 
  Integer静态成员变量
  	MAX_VALUE:整型的最大值
  	MIN_VALUE:整型的最小值
 */
public class IntegerDemo01 {
	public static void main(String[] args) {
		// 价格字符串
		String price = "100";
		System.out.println(Integer.parseInt(price));
		// 转换为整型
		Integer in = new Integer(price.trim());
		// 调用方法转基本数据类型
		int value = in.intValue();
		System.out.println(value);
		
		String str = "10.55";
		Double d = new Double(str);
		System.out.println(d.doubleValue());
		
		Long l = new Long("10000");
		System.out.println(l.longValue());
		System.out.println(100+"");
		System.out.println(Integer.toString(100) + 1);
	
		System.out.println(Integer.MAX_VALUE); 
		System.out.println(Integer.MIN_VALUE);
		
		System.out.println(Long.MAX_VALUE); 
		System.out.println(Long.MIN_VALUE);
		
		System.out.println(Double.MAX_VALUE); 
		System.out.println(Double.MIN_VALUE);
	}
}
```

## 10. 自动装箱和自动拆箱

- 自动装箱和自动拆箱概述
  - JDK1.5出现的新特性。


-  自动装箱的概念
  - 自动将基本数据类型转换成对应的包装类类型的过程。


-  自动拆箱的概念
  - 自动将包装类型转换成对应的基本数据类型的过程。


-  自动装箱和拆箱的好处
  -  基本数据类型和对应的包装类型直接参与运算。

```java
public class Demo01 {
	public static void main(String[] args) {
		// 基本数据类型
		int a = 100;
		// 创建包装类对象
		// Integer in = new Integer(a);
		// 自动装箱
		Integer in = 200; // 等价于 Integer in = Integer.valueOf(200);
		// 自动拆箱
		int b = in;  // 等价于 int b = in.intValue();
		// 自动装箱和拆箱
		Integer c = in + b;  //  Integer.valueOf(in.intValue() + b);
		System.out.println(c); 
		
		// 创建集合
		ArrayList<Integer> list = new ArrayList<>();
		list.add(1); // list.add(Integer.valueOf(1))
		list.add(2);
		list.add(new Integer(1));
		list.add(new Integer(3));
	}
}
```

## 11. 正则表达式的概念和作用

### 11.1 什么是正则表达式

- 一个定义匹配规则的字符串

### 11.2 正则表达式的作用

- 用来校验某一个字符串是否符合定义的规则。

### 11.3 正则表达式语法规则
* 字符
  ```java
  x   匹配字符X 
  \t  制表符
  \n  换行符
  \r  回车符 
  ```

* 字符类[记忆]
  ```java
  [abc] 	匹配abc中的任意一个	 
  [^abc]   匹配除abc以外的任意一个
  [a-zA-Z]  匹配所有的字母(大小写)的任意一个
  [0-9]     匹配所有数字的任意一个
  [a-zA-Z_0-9]  匹配所有数字，字母，下划线任意一个
  注意事项：中括号中可以有n个字符，但在匹配时只会匹配其中的一个。
  ```

* 预定义字符类[记忆]
  ```java
  .   	通配符，能够匹配(除\r和\n以外)任意一个字符
  \\d    等价[0-9] 匹配所有数字的任意一个
  \\w    等价[a-zA-Z_0-9]  匹配所有数字，字母，下划线任意一个
  \\s    匹配空白字符，\t  \n  \r
  ```

* 数量词[记忆]
  ```java
  X?    X出现0次或1次  0或1
  X*    X出现0次或多次，任意次
  X+    X出现1次或多次，至少一次
  X{n}  X刚好出现n次  
  X{n,} X至少要出现n次 
  X{n,m}  X至少要出现n次，至多出现m次， [n,m]
  ```

- 示例代码

```java
public class RegexDemo01 {
	/**
	 * 数量词
	 * @param args
	 */
	public static void main(String[] args) {
		// 字符串：定义规则的字符串
		// String regex = "a?"; // 0或1
		//String regex = "ba*b"; // 任意次
		// String regex = "a+"; // 至少一次
		// String regex = "a{5}"; // 刚好5次
		// String regex = "a{5,}"; // 至少5次
		String regex = "a{5,10}"; // 至少5次,至多10次
		// 字符串：被校验的字符串
		String str = "aaaa";
		// 校验str是否符合regex定义的规则
		boolean b = str.matches(regex);
		System.out.println(b);
	}
	
	/**
	 * 预定义字符
	 * @param args
	 */
	public static void test03() {
		// 字符串：定义规则的字符串
		// String regex = ".";
		//String regex = "\\d"; // 等价[0-9] 匹配所有数字的任意一个
		// String regex = "\\w"; // 等价[a-zA-Z_0-9] 
		String regex = "\\s"; // 匹配空白字符，\t  \n  \r
		// 字符串：被校验的字符串
		String str = "\r\n";
		// 校验str是否符合regex定义的规则
		boolean b = str.matches(regex);
		System.out.println(b);
	}
	
	/**
	 * 字符类:使用中括号括起一类字符，匹配时只会匹配其中任意一个
	 * @param args
	 */
	public static void test02() {
		// 字符串：定义规则的字符串
		// String regex = "[abc]";
		// String regex = "[^abc]";
		// String regex = "[0-9]";
		String regex = "[a-zA-Z_0-9]";
		// 字符串：被校验的字符串
		String str = "-";
		// 校验str是否符合regex定义的规则
		boolean b = str.matches(regex);
		System.out.println(b);
	}
	
	/**
	 * 字符：只能能匹配一个字符的字符串。等价equals方法比较
	 * @param args
	 */
	public static void test01(String[] args) {
		// 字符串：定义规则的字符串
		String regex = "a";
		// 字符串：被校验的字符串
		String str = "ab";
		// 校验str是否符合regex定义的规则
		boolean b = str.matches(regex);
		System.out.println(b);
	}
}
```

## 12. 正则表达式练习

### 12.1 匹配练习

* 检查手机号码是否合法
  * 规则：1开头，第二位可以是34578  后面9位数0-9
  * 正则表达式：1[34578]\\d{9}

* 检查QQ是否合法
  * 规则：0不能开头，全是数字，位数5到10位
  * 正则表达式： [1-9]\\d{4,9}

```java
public static void main(String[] args) {
		System.out.println("13634285612".matches("1[34578]\\d{9}"));
		System.out.println("132456732458".matches("[1-9]\\d{4,9}"));
}
```

### 12.2 切割练习

* 2016-10-24 按照-切割字符串
* 18 22 44 66 按照空格切割字符串 
* 192.168.1.204 按照点切割字符串

```java
public class RegexDemo03 {
	public static void main(String[] args) {
		// String[] strs = "2016-10-24".split("-");
		// String[] strs = "18 22 44 66".split("\\s");
		
		String[] strs = "192.168.1.204".split("\\.");
		System.out.println(strs.length);
		System.out.println(Arrays.toString(strs));
	}
}
```

### 12.3 替换练习
* 例子：
  * "Hello12345World6789012" 将所有数字替换为 “#”

```java
public class RegexDemo04 {
	public static void main(String[] args) {
		String str = "Hello12345World6789012";
		// 将所有数字替换为 “#”
		/**
		 * regex:正则规则字符串
		 * replacement:目标字符串
		 * 将满足正则规则的子串替换成目标字符串
		 */
		// String result = str.replaceAll("\\d", "#");
		String result = str.replaceAll("\\d+", "#");
		System.out.println(str);
		System.out.println(result);
	}
}
```

## 13. String类与正则相关的三个方法

```java
boolean matches(String 正则的规则)
  验证字符串是否符合正则规则 
String[] split(String 正则的规则)
  使用正则规则切割字符串
String replaceAll(String 正则规则,String 字符串)
  将字符串中符合正则规则的子串替换为后面的字符串
```
## 14. 验证邮箱正则表达式

```java
/**
  验证邮箱
 */
public class RegexDemo05 {
	public static void main(String[] args) {
		// 验证邮箱的规则
		// 12345@qq.com
		// pkxing@itcast.cn
		// 163233@sina.com.cn
		// 32435@163.com
		// (数字字母)+@(数字字母)+\\.(字母){2,}
		String regex = "[a-zA-Z1-9]\\w*@[a-zA-Z0-9]+(\\.[a-zA-Z]{2,})+";
		System.out.println("_@qq.com".matches(regex));
		System.out.println("pkxing@itcast.cn".matches(regex));
		System.out.println("pkxing@itcast.cn.com".matches(regex));
		System.out.println("x@163.cn".matches(regex));
		System.out.println("x@163.cn.edu.com".matches(regex));
		System.out.println("x@163.c".matches(regex));
	}
}
```


# 知识点回顾

- Date类

```java
Date(); 获得当前系统时间
Date(long date);根据毫秒值获得日期对象

long getTime();获得毫秒值
时间零点：从1970.1.1 00：00：00
毫秒值：1000毫秒 = 1秒
```

- DateFormat类

```java
是一个抽象类，用来将日期对象格式化成字符串，用来将字符串日期格式化为日期对象。
常用子类：SimpleDateFormat

SimpleDateFormat类构造方法
	SimpleDateFormat(String pattern); 根据日期默认创建格式化对象。

SimpleDateFormat类常用方法
	String format(Date date);将日期对象转换为字符串
	Date parse(String date); 将字符串转换为日期对象
	
日期模式
	yyyy
	MM
	dd
	HH 
	mm
	ss
```

- Calendar类

```java
是一个日历类，也是一个抽象类，不能直接创建对象。
如何创建日历类对象？
	通过Calendar类的静态方法：Calendar getInstance();

Calendar类常用方法
	int get(int field); 根据日历字段获得对应的值
	常用日历字段，都是Calendar类中的静态常量。
		YEAR
		MONTH
		DATE
		HOUR
		MINUTE
		SECOND
	Date getTime();获得日期对象
	long getTimeInMillis(); 获得毫秒值
	void add(int field,int value); 将指定日历字段的值加上一个偏移值。
	void set(int field,int value); 设置指定日历字段的值为指定的值
	void set(int year,int month,int date); 设置年月日
```

- System类

```java
long currentTimeMillis();获得当前系统时间的毫秒值
void gc(); 通知垃圾回收器回收垃圾。
void exit(int status) 退出JVM，终止程序运行。
void arraycopy(Object src,int srcPos,Object dest,int destPos,int length); 数组拷贝
Properties getProperties(); 获得操作系统的属性，比如操作系统名称。
```

- Math类

```java
double ceil(double d)
double floor(double d)
double sqrt(double b)
double pow(double a,double b)
double abs(double a)
double round(double d)
double random();
```

- 包装类

```java
int  Integer
char Character
其他的六种基本数据类型的包装是首字母大写即可。
```

- 自动装箱和拆箱

```
自动将基本数据类型转换为对应的包装类型的过程称为自动装箱。
自动将包装类型转换为对应的基本数据类型的过程称为自动拆箱。
```

- 正则表达式

  ```java
  字符
  	x
  	
  字符类
  	[abc]
  	[^abc]
  	[a-z]
  	[0-9]
  	[a-zA-Z_0-9]
  	
  预定于字符
  	. 通配符
  	\d  等价[0-9]
  	\w  等价[a-zA-Z_0-9]
  	\s  空白符

  数量词
  	*   任意次
      ？   0或1次
      +    至少1次
      {n}  刚好n次
      {n,}  至少n次
  	{n,m}  至少n次，至多m次
  ```

# 学习目标

- 说出Collection集合的常用功能
- 能够使用迭代器对集合进行取元素
- 能够说出集合的使用细节
- 能够使用集合存储自定义类型
- 能够使用foreach循环遍历集合
- 能够使用泛型定义集合对象
- 能够阐述泛型通配符的作用
- 能够完成斗地主案例		

# 今日内容

- 迭代器
- 增强for
- 泛型
- 斗地主案例

## 1. Collection基本方法
```java
public boolean add(E e)  添加元素
public boolean remove(Object obj) 删除元素obj 删除成功返回true
public void clear() 清空集合
public int size()   获得集合元素个数
public boolean contains(Object o) 判断集合中是否包含指定的元素o
public boolean isEmpty()  判断集合是否空。
```

## 2. 迭代器的概述和使用
### 2.1 什么是Iterator(迭代器)

- 一个用来遍历集合的对象，该对象的数据类型是：Iterator，是一个接口类型。

### 2.2 Iterator的好处

- 屏蔽了众多集合的内部实现方式，对外提供统一的遍历方式。
- 所有的单列集合都可以使用迭代器遍历。

### 2.3 迭代器的执行过程

```java
* boolean hasNext() 判断当前指针指向的位置是否有元素，如果有返回true，否则返回false
* E	next()  获得当前指针指向位置的元素，并将指针下移指向下一个元素。
```

![](image01.png)

### 2.4 示例代码

```java
public class IteratorDemo01 {
	public static void main(String[] args) {
		// 创建集合对象
		ArrayList<String> list = new ArrayList<>();
		list.add("a");
		list.add("b");
		list.add("c");
		list.add("d");
		list.add("e");
		// 遍历集合
		/**
		for (int i = 0; i < list.size(); i++) {
			// 根据索引i获得元素
			String str = list.get(i);
			System.out.println(str);
		} **/
		// 获得迭代器对象
		Iterator<String> it = list.iterator();
		// 使用循环改造
		while(it.hasNext()) { 
			System.out.println(it.next()); 
	    }
		
		/*System.out.println(it.next()); // a
		
		System.out.println(it.hasNext());  // true
		System.out.println(it.next()); // b
		
		System.out.println(it.hasNext());  // true
		System.out.println(it.next()); // c
		
		System.out.println(it.hasNext());  // true
		System.out.println(it.next()); // d
		
		System.out.println(it.hasNext());  // false
		System.out.println(it.next()); // d
*/		
	}
}
```

## 3. ArrayList迭代器源码分析(迭代原理)

## 4. 迭代自定义对象
### 4.1 案例演示

* 迭代自定义Person对象，将所有人的年龄相加求和。 

```java
public class IteratorDemo02 {
	public static void main(String[] args) {
		// 创建集合
		ArrayList<Student> list = new ArrayList<>();
		list.add(new Student("jack",20));
		list.add(new Student("rose",18));
		list.add(new Student("lily",19));
		list.add(new Student("laowang",30));
		
		// 遍历集合
		// 定义变量用来统计年龄之和
		int sum = 0;
		// 获得迭代器
		// Iterator<Student> it =  list.iterator();
		// 获得ListIterator对象
		ListIterator<Student> it =  list.listIterator();
		while(it.hasNext()) {
			// 获得学生对象
			Student stu = it.next();
			// 累加年龄
			sum += stu.getAge();
		}
		System.out.println("总年龄：" + sum);
	}
}

class Student{
	private String name;
	private int age;
	public Student(String name, int age) {
		super();
		this.name = name;
		this.age = age;
	}
	public Student() {
		super();
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
	@Override
	public String toString() {
		return "Student [name=" + name + ", age=" + age + "]";
	}
	
}
```

### 4.2 并发修改异常演示

* 迭代集合中的Person对象，当Person的年龄为18时，再向集合中添加一个90岁的人。 

```Java
public class IteratorDemo02 {
	public static void main(String[] args) {
		// 创建集合
		ArrayList<Student> list = new ArrayList<>();
		list.add(new Student("jack",20));
		list.add(new Student("rose",18));
		list.add(new Student("lily",19));
		list.add(new Student("laowang",30));
		
		// 遍历集合
		// 定义变量用来统计年龄之和
		int sum = 0;
		// 获得迭代器
		// Iterator<Student> it =  list.iterator();
		// 获得ListIterator对象
		ListIterator<Student> it =  list.listIterator();
		while(it.hasNext()) {
			// 获得学生对象
			Student stu = it.next();
			// 累加年龄
			sum += stu.getAge();
			// 判断姓名是否是lily
			if(stu.getName().equals("lily")) {
				// 添加一个新的学生
				// list.add(new Student("lucy",19));
				
				// 调用ListIterator迭代器的add方法添加元素
				it.add(new Student("lucy",19));
			}
		}
		System.out.println("总年龄：" + sum);
		System.out.println(list.size()); // 5
	}
}
```

### 4.3 并发修改异常解决办法

```java
* 使用普通for循环遍历。(for(int index = 0;index < 集合.size();index ++))
* 使用listItreator迭代器进行迭代,在遍历过程中通过迭代器的add方法添加元素，不能通过集合对象的方法添加元素。 
```

## 5. 增强for
### 5.1 增强for概述

```java
增强for的概述
	* 概念：JDK1.5的新特性
	* 作用：专门用来遍历集合和数组
	* 本质：就是迭代器，一样存在并发修改的问题

增强for的格式
	for(数据类型 变量名:数组名或集合名){
		
	}
增强for使用注意事项
	* 如果集合或数组中存储的是引用类型的数据，则在增强for中使用引用变量修改对象的属性值会影响集合或数组中对象的属性中，因为修改的是同一个内存空间的值。
```

### 5.2 增强for格式

```java
for(数据类型 变量名:数组名或集合名){
    
}
```

### 5.3 代码演示

* 增强for循环遍历数组

```Java
public class ForEachDemo {
	public static void main(String[] args) {
		// 整型数组
		int[] arr = {1,2,3,4,5,6};
		/*
		 for(数据类型 变量名:数组名或集合名){
			
		}
		 */
		for(int num : arr) {
			num++;
			System.out.println(num);  // 2,3,4,5,6,7
		}
		
		for(int num : arr) {
			System.out.println(num);  // 1,2,3,4,5,6
		}
	}
}
```

* 增强for循环遍历集合

```java
public class ForEachDemo {
	public static void main(String[] args) {
		// 创建集合
		ArrayList<Student> list = new ArrayList<>();
		list.add(new Student("jack",20));
		list.add(new Student("rose",18));
		list.add(new Student("lily",19));
		list.add(new Student("laowang",30));
		// 遍历集合
		for(Student stu: list) {
			System.out.println(stu);
			stu.setAge(100);
		}
		System.out.println("-------------------");
		for(Student stu: list) {
			System.out.println(stu);
		}
	}
}
```

## 6. 集合中泛型的使用
### 6.1 集合中存在什么样的安全隐患

* 集合默认可以存储任意类型的对象。
* 当在存储String的集合中，存储一个Integer类型，调用String类型的特有方法就会报错，导致程序崩溃。

### 6.2 集合中泛型的使用

* 创建集合使用泛型指定集合只能存放的数据类型。
* 遍历集合时不需要进行类型转换。

### 6.3 泛型的好处(优点)

* 增强了集合安全性，把运行时的错误转为编译时错误。
* 省去了类型强转的麻烦。

#### 6.4 Java中的伪泛型

* 泛型只在编译时存在，编译后就被擦除，在编译之前我们就可以利用泛型限制集合存储对象的类型。

### 6.5 集合中使用泛型注意事项

* 在泛型中没有多态的概念，要不左右两边的数据类型要保持一致，要不只写一边。推荐两边都写一样的。
* 泛型不准使用基本数据类型，如果需要使用基本数据类型，要使用基本数据类型对应的包装类。 

### 6.6 示例代码

```java
public class Demo01 {
	public static void main(String[] args) {
		// 创建集合对象
		ArrayList<String> list = new ArrayList<>();
		list.add("abc");
		list.add("bcd");
		list.add("def");
		// list.add(123);
		
		/*// 遍历集合
		for (Object object : list) {
			// 向下转型
			String str = (String)object;
			// 调用子类特有的方法
			System.out.println(str.length());
		}*/
		for (String str : list) {
			System.out.println(str.length());
		}
	}
}
```

## 7. 泛型方法

```java
泛型的概述
	* JDK1.5新特性
    * 一般使用在方法上，类上，接口上。
    * 泛型变量可以理解为是一种数据类型的占位符。
    * 泛型变量也可以理解为某种数据类型的变量。
    * 泛型变量的命名规则：一般是使用大小写，常用的字母：T，E,K,V
    * 泛型变量不能使用基本数据类型，如果要使用要使用对应包装类类型。
```

### 7.1 泛型方法的引入

* 定义一个方法接收一个任意类型的参数，返回值类型与实际参数类型一致。 


### 7.2 什么是泛型方法

- 在方法声明上使用了泛型变量的方法就是泛型方法

### 7.3 泛型方法的格式

```java
修饰符 <泛型变量> 返回值类型 方法名(参数列表) {
}
```

### 7.4 泛型方法注意事项

- 泛型方法上泛型变量的具体数据类型在调用方法时由参数类型决定。

## 8. 泛型类的定义和使用
### 8.1 泛型类的引入

```java
* 定义一个数组工具类MyArrays
* 提供两个成员方法，方法参数接收一个任意类型的数组
 	* 一个方法的功能是将数组的元素反转.
	* 一个方法的功能是将数组的元素拼接成一个字符串返回。
		* 字符串格式："[1,2,3,4,5]" 
    
public class Demo03 {
	public static void main(String[] args) {
		// 整型数组
		Integer[] arr = {1,2,3,4,5};
		// 字符串数组
		String[] strs = {"a","b","c"};
		// 创建MyArrays对象
		MyArrays<String> my = new MyArrays<String>();
		my.reverse(strs);
		System.out.println(my.toString(strs));
		
		// 创建MyArrays对象
		MyArrays<Integer> my1 = new MyArrays<>();
		my1.reverse(arr);
		System.out.println(my1.toString(arr));
		
		System.out.println(MyArrays.toString1(arr));
		System.out.println(MyArrays.toString1(strs));
	}
}

class MyArrays<T>{
	/**
	 * 反转数组元素
	 */
	public void reverse(T[] arr) {
		for(int i = 0,j = arr.length - 1;i < j; i++,j--) {
			T temp = arr[i];
			arr[i] = arr[j];
			arr[j] = temp;
		}
	}
	/**
	 * 将数组元素拼接成指定的字符串
	 */
	public String toString(T[] arr) {
		StringBuilder sb = new StringBuilder("[");
		for (T t : arr) {
			sb.append(t).append(",");
		}
		sb.deleteCharAt(sb.length()-1);
		sb.append("]");
		return sb.toString();
	}
	
	/**
	 * 静态方法：泛型方法
	 */
	public static <T> String toString1(T[] arr) {
		StringBuilder sb = new StringBuilder("[");
		for (T t : arr) {
			sb.append(t).append(",");
		}
		sb.deleteCharAt(sb.length()-1);
		sb.append("]");
		return sb.toString();
	}
}
```

### 8.2 泛型类的概念

- 在类定义时使用了泛型变量的类就是泛型类。

### 8.3 泛型类的定义格式

```Java
public class 类名<泛型变量>{
    
}
```

### 8.4 泛型类注意事项

- 在类上定义的泛型其具体数据类型是在使用该类创建对象时指定的。


- 在使用泛型类创建对象时如果没有指定泛型的具体数据类型，那么默认的数据类型就是Object。


- 静态方法不能使用类上定义的泛型变量，如果需要使用需要将该方法定义为泛型方法。

### 8.5 查看官方泛型类ArrayList的定义和使用

```java
// ArrayList泛型类的定义
class ArrayList<E>{ 
	public boolean add(E e){ }
	public E get(int index){  }
}
// 创建对象时，确定泛型的类型
例如，ArrayList<String> list = new ArrayList<String>();
此时，变量E的值就是String类型
class ArrayList<String>{
	public boolean add(String e){ }
	public String get(int index){  }
}
例如，ArrayList<Integer> list = new ArrayList<Integer>();
此时，变量E的值就是Integer类型
class ArrayList<Integer>{ 
	public boolean add(Integer e){ }
	public Integer get(int index){  }
}
```
## 9. 泛型接口

### 9.1 什么是泛型接口

- 在接口定义时使用了泛型变量的接口就是泛型接口。

### 9.2 泛型接口的定义格式	

```java
public interface 接口名<泛型变量>{
 			
}
```

### 9.3 如何实现泛型接口

- 实现接口的同时指定泛型变量的具体数据类型。


-  实现接口的时候不指定泛型变量的具体数据类型，将该类定义泛型类，由实现类使用者在创建该类对象时指定泛型变量的具体数据类型。(推荐使用，比较灵活) 

### 9.4 泛型接口定义和实现演示

```java
/**
 * 数据访问层：直接跟数据库打交道，对数据库进行增删改查操作
 */
public interface Dao<T> {
	public void save(T t);
	public void update(T t);
	public void delete(int id);
	public T findById(int id);
}

/**
 * 实现接口的同时指定泛型变量的具体数据类型。
 */
public class StudentDao implements Dao<Student> {
	@Override
	public void save(Student t) {
		
	}

	@Override
	public void update(Student t) {
		
	}
	@Override
	public void delete(int id) {
		
	}
	@Override
	public Student findById(int id) {
		
		return null;
	}
}
/**
 * 实现接口的同时指定泛型变量的具体数据类型。
 */
public class ProductDao implements Dao<Product> {
	@Override
	public void save(Product t) {
		
	}

	@Override
	public void update(Product t) {

	}

	@Override
	public void delete(int id) {
	
	}

	@Override
	public Product findById(int id) {
		
		return null;
	}
}

/**
 * 通用Dao
 * @author pkxing
 *
 */
public class BaseDao<T> implements Dao<T> {
	@Override
	public void save(T t) {
	}

	@Override
	public void update(T t) {
	}

	@Override
	public void delete(int id) {
	}

	@Override
	public T findById(int id) {		
		return null;
	}
}

public class Demo04 {
	public static void main(String[] args) {
		/*
		// 创建StudentDao对象
		StudentDao stuDao = new StudentDao();
		stuDao.save(new Student());
		
		// 创建ProductDao对象
		ProductDao pd = new ProductDao();
		pd.save(new Product());
		*/
		
		// 创建BaseDao对象
		BaseDao<Student> stuDao = new BaseDao<>();
		stuDao.save(new Student());
		
		// 创建BaseDao对象
		BaseDao<Product> pd = new BaseDao<>();
		pd.save(new Product());
		
	}
}
```
### 9.5 查看官方泛型接口List的定义

```java
 /*
  *  带有泛型的接口
  *  
  *  public interface List <E>{
  *    abstract boolean add(E e);
  *  }
  * 
  *  实现类先实现接口,不理会泛型
  *  public class ArrayList<E> implements List <E>{
  *  }
  *  调用者 : new ArrayList<String>() 后期创建集合对象的时候,指定数据类型
```

## 10. 泛型上下限
### 10.1 泛型上下限引入

```java
需求1
  - 定义一个方法可以接收任意类型的集合对象
  - 集合对象只能存储Integer或者是Integer的父类数据。
需求2
  - 定义一个方法可以接收任意类型的集合对象
  - 集合对象只能存储Number或者是Number的子类数据。
```

### 10.2 泛型上下限概述

```java
泛型的上下限概述和使用(了解)

泛型通配符
    ?  是一个通配符，匹配任意一种数据类型。

泛型上下限的引入
    需求1
        - 定义一个方法可以接收任意类型的集合对象
        - 集合对象只能存储Integer或者是Integer的父类数据。

    需求2
        - 定义一个方法可以接收任意类型的集合对象
        - 集合对象只能存储Number或者是Number的子类数据。

泛型上限
    ? extends Number：是上限，只能接受Number或Number的子类类型

泛型下限
    ? super Integer ：是下限，只能接受Integer或Integer的父类类型

泛型上下限的注意事项
    * 通配符?一般不会单独使用，一般都会结合泛型的上下限使用。
    * 通配符?不能用来定义泛型方法,泛型类以及泛型接口。
```

- 演示代码

```java
public class Demo05{
	public static void main(String[] args) {
		ArrayList<Number> list01 = new ArrayList<>();
		ArrayList<Integer> list02 = new ArrayList<>();
		ArrayList<Object> list03 = new ArrayList<>();
		print01(list01);
		print01(list02);
		print01(list03);
		
		
		print02(list01);
		print02(list02);
		// print02(list03);
		
	}
	
	/**
	 *	需求2
  			- 定义一个方法可以接收任意类型的集合对象
  			- 集合对象只能存储Number或者是Number的子类数据。
	 * @param list
	 */
	public static void print02(ArrayList<? extends Number> list) {
		
	}
	
	/**
	 * - 定义一个方法可以接收任意类型的集合对象
  	   - 集合对象只能存储Integer或者是Integer的父类数据。
	 * @param list
	 */
	public static void print01(ArrayList<? super Integer> list) {
		
	}
}
```

## 11. 集合综合案例---斗地主洗牌发牌(不排序)

```java
/**
 11. 集合综合案例---斗地主洗牌发牌(不排序)
	组牌
	洗牌
	发牌
	看牌
 */
public class DDZDemo {
	public static void main(String[] args) {
		// 组牌：四种花色+13个数字+大小王
		// 创建集合用来存储牌
		ArrayList<String> pokers = new ArrayList<>();
		// 花色
		String[] colors = {"♦️","♣️","♥️","♠️"};
		// 数字
		String[] nums = {"3","4","5","6","7","8","9","10","J","Q","K","A","2"};
		// 遍历数字
		for (String num : nums) {
			// 遍历花色
			for (String color : colors) {
				// 拼接牌
				// String poker = color+num;
				String poker = color.concat(num);
				// 将牌添加到集合中
				pokers.add(poker);
			}
		}
		// 添加大小王
		pokers.add("🃏");
		pokers.add("🤴");
		System.out.println(pokers);
		// 洗牌:shuffle将集合中的元素顺序打乱
		Collections.shuffle(pokers);
		System.out.println(pokers);
		// 准备玩家集合
		ArrayList<String> player01 = new ArrayList<>();
		ArrayList<String> player02 = new ArrayList<>();
		ArrayList<String> player03 = new ArrayList<>();
		// 底牌集合
		ArrayList<String> dp = new ArrayList<>();
		// 发牌
		for (int index = 0; index < pokers.size(); index++) {
			// 根据索引获得对应的牌
			String poker = pokers.get(index);
			if(index < 3) {
				dp.add(poker);
			} else if(index % 3 == 0) {
				player01.add(poker);
			} else if (index % 3 == 1) {
				player02.add(poker);
			} else {
				player03.add(poker);
			}
		}
		// 查看牌
		System.out.println(dp);
		System.out.println(player01);
		System.out.println(player02);
		System.out.println(player03);
	}
}
```

## 12. 小结

- 迭代器概述

  - 一个用来遍历集合的对象。
  - 通过调用集合对象的iterator方法获得迭代器对象。

- 迭代器常用方法

  ```java
  boolean hasNext(); 判断当前指针指向的位置是否有元素，有返回true，否则返回false、
  E next(); 返回当前指针指向位置的元素，并将指针下移一个位置。
  ```

- 并发修改异常的解决方案

  ```
  1.使用普通for循环遍历
  2.使用ListIterator来遍历，调用ListIterator接口中方法来增删元素。
  ```

- 增强for的作用

```
JDK1.5的新特性。专门用来遍历集合和数组的
```

- 增强for格式

```java
for(数据类型 变量名：数组名或集合名){
    
}
```

- 泛型方法的定义

```java
修饰符 <泛型变量> 返回值类型 方法名(参数列表) {
    
}
* 泛型变量在调用方法时确定具体的数据类型，如果没有指定则默认是Object
```

- 泛型类的定义

```java
public class 类名<泛型变量>{
    
} 
* 泛型变量在创建对象时确定具体的数据类型，如果没有指定默认是Object
* 静态方法上不能使用类上定义的泛型变量，如果要使用，则该方法需要定义为泛型方法。
```

- 泛型接口的定义和实现

```java
public interface 接口名<泛型变量>{
    
} 
* 实现接口同时指定泛型变量的具体数据类型
public class Xxx implemants 接口名<具体数据类型>{
    
} 
* 实现接口时不指定泛型变量的具体数据类型(推荐使用，比较灵活)
public class Xxx<泛型变量> implemants 接口名<泛型变量>{
    
} 
```


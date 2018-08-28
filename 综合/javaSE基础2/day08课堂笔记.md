# 学习目标

- 能够说出List集合的特点				
- 使用List存储的数据结构
- 能够说出List常见的三个的特点
- 能够说出Set集合的特点
- 说出哈希表的特点
- 使用HashSet集合存储自定义元素
- 说出判断集合元素唯一的原理	

# 今天内容

- 数据结构
- 单列集合
- 哈希表

## 1. 数据的存储结构

- 栈

  - 先进后出，后进先出，First In Last Out(FILO)

- 队列

  - 先进先出 First In First Out(FIFO)

- 数组

  - 查询快：直接根据索引获得元素。
  - 增删慢：增加或删除元素需要移动大量元素的位置。

- 链表

  - 查询慢：每次查询从链表头或链表尾部开始遍历查询
  - 增删快：增加或删除元素不需要移动元素的位置，只需要修改上一个元素记住下一个元素的地址。
  - 查询的索引值决定了从表头还是表尾开始查询
    - 如果索引值小于元素个数一半时，从表头开始查找。
    - 如果索引值大于等于元素个数一半时，从表尾开始查找。

  ![](image02.png)

## 2. 单列集合体系

![](image01.png)

## 3. List接口概述
### 3.1 特点

- 有索引
- 有顺序：存取顺序一致
- 元素可重复

### 3.2 List集合遍历方式

- 普通for
- 增强for
- 迭代器

### 3.3 List接口的常用子类

- ArrayList
- LinkedList

### 3.4 List常用方法演示

* 增删改查方法 

## 4. ArrayList集合的特点

- 底层是数组结构


- 查询快，增删慢。
- 线程不安全，效率高。

## 5. LinkedList特有方法

```java
 LinkedList栈结构的相关方法
 	void push(E e):将元素压入栈中。
 	E pop():将栈顶元素出栈，从集合中删除。
 	E peek(): 获得栈顶元素，并不删除
 	
 LinkedList队列结构的相关方法
 	void offer(E e) 添加元素到队列中
 	E poll() 获得队列头部元素，并从队列中删除。
 	E peek(): 获得队列头部元素，并不删除

LinkedList链表特有方法
	addFirst(E) 将元素添加到链表头
	addLast(E)  将元素添加到链表尾
	E getFirst() 获得链表头元素
	E getLast()  获得链表尾元素
	E removeFirst() 移除链表头部元素
	E removeLast()  移除链表尾部元素
```

- 示例代码

```java
public class LinkedListDemo {
	public static void main(String[] args) {
		// LinkedList栈结构的相关方法
		test01();
		// LinkedList队列结构的相关方法
		test02();
		// LinkedList链表结构特有方法
		test03();
	}
	
	/**
	 * LinkedList链表结构特有方法
	 */
	public static void test03() {
		// 创建集合对象
		LinkedList<String> list = new LinkedList<>();
		// 将元素添加到链表头
		list.addFirst("a");
		list.addFirst("b");
		list.addFirst("c");
		
		// 将元素添加到链表尾部
		list.addLast("d");
		list.addLast("e");
		list.addLast("f");
		
		// 删除链表头元素
		list.removeFirst();
		// 删除链表尾元素
		list.removeLast();
		
		// 获得链表头元素
		System.out.println(list.getFirst());
		// 获得链表尾元素
		System.out.println(list.getLast());
		
		System.out.println(list);
	}
	
	/**
	 * LinkedList队列结构的相关方法
	 */
	public static void test02() {
		// 创建集合对象
		LinkedList<String> list = new LinkedList<>();
		// 添加元素到队列中
		list.offer("a");
		list.offer("b");
		list.offer("c");
		
		// 获得队列头部的元素
		String str1 = list.poll();
		System.out.println(str1); // a
		String str2 = list.poll();
		System.out.println(str2); // b
		
		System.out.println(list.peek()); // c
		System.out.println(list); // c
	}
	
	/**
	 * LinkedList栈结构的相关方法
	 */
	public static void test01() {
		// 创建集合对象
		LinkedList<String> list = new LinkedList<>();
		// 添加元素:压栈
		list.push("a");
		list.push("b");
		list.push("c");
		
		// pop():将元素出栈
		String str1 = list.pop();
		System.out.println(str1);
		
		// peek():获得栈顶元素，并不删除
		String str2 = list.peek();
		System.out.println(str2);
		
		System.out.println(list);
		/*for (String str : list) {
			System.out.println(str);
		}*/
	}
}
```

## 6. LinkedList集合的特点

- 底层是链表结构
- 增删快
- 查询慢
- 线程不安全的，效率高。

## 7. 如何选择ArrayList和LinkedList

- 如果需要大量增删元素时，则建议使用LinkedList
- 如果只是作查询操作，则建议使用ArrayList

## 8. Set接口的概述
### 8.1 Set接口的特点

- 无序
- 无索引
- 元素不可重复

### 8.2 Set集合遍历方式	

- 迭代器
- 增强for

### 8.3 Set集合常用子类

- HashSet
- LinkedHashSet

## 9. HashSet的特点和使用
### 9.1 HashSet的特点

- 无序，无索引，元素不可重复
- 底层是哈希表

### 9.2 什么是哈希表

- 哈希表就是数组和链表的结合体。

### 9.3 哈希表特点

- 查询和增删都比较快。

### 9.4 代码演示

```java
/**
  HashSet常用方法
  	boolean add(E e);  添加成功返回true，否则返回false。
  	boolean remove(Object o); 删除指定元素，成功返回true，否则返回false。
 */
public class HashSetDemo01 {
	public static void main(String[] args) {
		// 创建HashSet集合
		HashSet<String> set = new HashSet<>();
		// 添加元素
		set.add("a");
		set.add("b");
		set.add("d");
		set.add("a");
		
		// 删除元素
		// System.out.println(set.remove("a")); // true
		
		// 增强for遍历集合
		for (String str : set) {
			System.out.println(str);
		}
		// 迭代器遍历
		Iterator<String> it = set.iterator();
		while(it.hasNext()) {
			System.out.println(it.next());
		}
		System.out.println(set);
		
		/*HashSet<Integer> list = new HashSet<>();
		list.add(1);
		list.add(2);
		list.add(4);
		list.add(3);
		System.out.println(list.remove(1));
		System.out.println(list);*/
	}
}
```

## 10. LinkedHashSet特点和使用

- 继承HashSet，能够保证存取有顺序。

```java
/**
 * LinkedHashSet的基本演示
 */
public class LinkedHashSetDemo04 {
	public static void main(String[] args) {
		// 创建HashSet集合
		HashSet<String> set = new LinkedHashSet<>();
		// 添加元素
		set.add("dfas");
		set.add("xxo");
		set.add("45tfcdss");
		set.add("hjk");
		set.add("hjk");
		set.add("5678");
		System.out.println(set);
	}
}
```

## 11. 对象的哈希值

```java
对象的哈希值概述
 	* 就是一个十进制整数
 	* 通过调用对象的hashCode方法获得对象的哈希值。
 	* 默认哈希值就是对象在内存中的地址值。
 		
对象哈希值的作用
 	* 它是对象存储到哈希表的依据。
Object类的常用方法	 
 	int	hashCode(); 获得对象的哈希值。
```

- 示例代码

```java
public class HashCodeDemo {
	public static void main(String[] args) {
		// 创建学生对象
		Student stu1 = new Student("jack",23);
		Student stu2 = new Student("jack",23);
		
		// 获得对象的哈希值
		int value1 = stu1.hashCode();
		int value2 = stu2.hashCode();
		
		System.out.println(stu1);
		System.out.println(stu2);
		
		System.out.println(value1);
		System.out.println(value2);
		System.out.println("----------------------------");
		System.out.println(new String("abc").hashCode());
		System.out.println(new String("abc").hashCode());
		// 创建集合
		HashSet<String> set = new HashSet<>();
		set.add(new String("abc"));
		set.add(new String("abc"));
		set.add("def");
		System.out.println(set);
		
	}
}
```

## 12. 哈希表的存储过程

* 哈希表的存储过程(存取原理)：每存入一个新的元素都要走以下五步
  * (1)调用对象的hashCode()方法，获得要存储元素的哈希值。
  * (2)将哈希值与表的长度(即数组的长度)进行求余运算得到一个整数值，该值就是新元素要存放的位置(即是索引值)。
    * 如果索引值对应的位置上没有存储任何元素，则直接将元素存储到该位置上。
    * 如果索引值对应的位置上已经存储了元素，则执行第3步。
  * (3)遍历该位置上的所有旧元素，依次比较每个旧元素的哈希值和新元素的哈希值是否相同。
       * 如果有哈希值相同的旧元素，则执行第4步。
       * 如果没有哈希值相同的旧元素，则执行第5步。
  * (4)比较新元素和旧元素的地址是否相同
    * 如果地址值相同则用新的元素替换老的元素。停止比较。
    * 如果地址值不同，则新元素调用equals方法与旧元素比较内容是否相同。
        * 如果返回true，用新的元素替换老的元素，停止比较。
        * 如果返回false，则回到第3步继续遍历下一个旧元素。
   * (5)说明没有重复，则将新元素存放到该位置上并让新元素记住之前该位置的元素。
* 示例代码

```java
/**
  哈希表存储元素过程
 */
public class HashSetDemo02 {
	public static void main(String[] args) {
		// 创建HashSet集合
		HashSet<String> set = new HashSet<>();
		// 添加元素
		set.add("abc"); // 96354 % 16 = 2 
		set.add("abc"); 
		set.add("aaa");
		set.add("bbb");
		System.out.println(set);
	}
}
```

## 13. HashSet存储自定义对象
* 需求
  * 定义一个学生类，包含姓名和身份证号码 
  * 创建多个Person对象存储到HashSet集合中。
  * 要求属性值完全相同的对象只存储一个。

* 哈希表的存储自定义对象
  * 默认对象的hashCode是从Object中继承的
  * Object的hashCode是根据内存地址计算出来的
  * 所以只要对象的地址不同，hashCode就不会相同。
  * 所以就算两个对象属性值完全相同，他们两个也不会判定为重复元素。

* 自定义对象重写hashCode和equals方法
  * 重写自定义对象的hashCode方法

    ​	尽可能让不同的属性值产生不同的哈希值，这样就不用再调用equals方法去比较属性，效率比较高。

* 小结
  * 当使用HashSet存储自定义类型，如果没有重写该类的hashCode与equals方法，则判断是否重复的依据是根据对象地址值，如果想通过内容比较元素是否相同，需要重写该类的hashcode与equals方法。

* 示例代码

```java
public class Student {
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

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + age;
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Student other = (Student) obj;
		if (age != other.age)
			return false;
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		return true;
	}
}

/**
  哈希表存储自定义对象
	- 定义一个学生类，包含姓名和身份证号码 
	- 创建多个Student对象存储到HashSet集合中。
	- 要求属性值完全相同的对象只存储一个。
 */
public class HashSetDemo03 {
	public static void main(String[] args) {
		// 创建HashSet集合
		HashSet<Student> set = new HashSet<>();
		// 添加元素
		set.add(new Student("jack",23));  // 324543 % 16 = 1
		set.add(new Student("jack",23));  // 324543 % 16 = 1
		
		
		set.add(new Student("rose",24));  // 564534 % 16 = 1
		set.add(new Student("rose",24));  // 564534 % 16 = 1
		
		System.out.println(set);
		System.out.println(set.size());
	}
}
```

## 14. ArrayList判断对象是否重复的原理
* 需求
  * 定义一个学生类，包含姓名和身份证号码 
  * 创建三个学生对象学生添加到ArrayList集合中。
  * 要求：
    * 有两个学生的姓名和身份证是一样的。 
    * 相同姓名和身份证号的学生只存储一个。
* **ArrayList的contains方法判断元素是否重复原理。**
   * 调用传入元素的equals方法依次与集合中的旧元素所比较，从而根据返回的布尔值判断是否有重复元素。
   * 当ArrayList存放自定义类对象时，当自定义类在未重写equals方法前，判断是否重复的依据是地址值。如果想根据内容判断是否为重复元素，则需要重写自定义类的equals方法。
   * **底层依赖于equals()方法。**
* 示例代码
```java
/**
  ArrayList存储自定义对象
	- 定义一个学生类，包含姓名和身份证号码 
	- 创建多个Student对象存储到ArrayList集合中。
	- 要求属性值完全相同的对象只存储一个。
 */
public class ArrayListDemo02 {
	public static void main(String[] args) {
		// 创建HashSet集合
		ArrayList<Student> list = new ArrayList<>();
		
		// 添加元素
		// list.add(new Student("jack",23)); 
		// list.add(new Student("jack",23));  
		
		// list.add(new Student("rose",24));  
		// list.add(new Student("rose",24));
		
		addObject(list, new Student("jack",23));
		addObject(list, new Student("jack",23));
		addObject(list, new Student("rose",24));
		addObject(list, new Student("rose",24));
		
		System.out.println(list);
		System.out.println(list.size());
	}
	
	/**
	 * 添加指定的对象到list集合中
	 * @param list
	 * @param stu
	 */
	public static void addObject(ArrayList<Student> list,Student stu) {
		// 判断集合中是否包含stu对象
		if(list.contains(stu)) {
			System.out.println("进来了吗");
			return;
		}
		list.add(stu);
	}
}
```

## 15. HashSet判断对象是否重复的原理
* **HashSet的add/contains等方法判断元素是否重复原理**
  * 将新元素与集合中已有的旧元素的HashCode值进行比较。
    * 哈希值相同，再通过比较地址值或equals方法比较内容是否相同。
      * 只要地址或内容相同，则表示包含。否则表示不包含。
    * 哈希值不同，直接返回false表示不包含。
  * **底层依赖于hashCode()和equals()。**


* **HashSet构造方法分析**
   * 比如：加载因子是**0.75**，数组的长度为**16**，其中存入**16 * 0.75 = 12**个元素。如果再存入第十三个(>12)元素。那么此时会扩充**哈希表(再哈希**)，底层会开辟一个长度为**原长度2倍的数组**。把老元素拷贝到新数组中，再把新元素添加数组中。**当存入元素数量 > 哈希表长度 * 加载因子**，就要扩容，因此加载因子决定扩容时机。

## 16. hashCode和equals方法面试题
* hashCode和equals的面试题
   * 两个Person对象p1和p2，如果p1.hashCode() == p2.hashCode()，则p1.equals(p2)一定是true吗？

     不一定

   * 两个Person对象p1和p2，如果p1.equals(p2)为true。则 p1.hashCode() == p2.hashCode()一定是true吗？


     一定


* hashCode的官方协定
  1. 如果根据equals(Object)方法，两个对象是相等的。那么对这两个对象中的每个对象调用hashCode方法都必须生成相同的整数结果。 
  2. 如果根据equals(java.lang.Object)方法，两个对象不相等，那么对这两个对象中的任一对象上调用 hashCode 方法`不` 要求一定生成不同的整数结果。但是，程序员应该意识到，为不相等的对象生成不同整数结果可以提高哈希表的性能。


## 17. 小结

- 单列集合
  - List：有序，有索引，元素可重复
    - ArrayList：有序，有索引，元素可重复，底层是数组，查询快，增删慢，线程不安全效率高。
    - LinkedList：有序，有索引，元素可重复，底层是链表，查询慢，增删快，线程不安全效率高。
  - Set：无序，无索引，元素不可重复
    - HashSet：底层是哈希表，增删和查询都比较快。
    - LinkedHashSet：继承HashSet，能够保证存取顺序一致。
- 如何选择集合
  - 如果要求元素可以重复，则在List接口下选择
  - 如果要求元素不可重复，则在Set接口下选择
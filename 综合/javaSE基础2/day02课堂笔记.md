# 知识点回顾

- 匿名对象的概述
  - 没有引用变量指向的对象就是匿名对象(没有名字的对象)
  - 格式：new 类名();
- 匿名对象的特点
  - 使用完毕之后就立即变成垃圾对象，会在垃圾回收器空闲的时候进行回收。
- 匿名对象的使用场景
  - 作为实际参数传递
  - 作为方法的返回值
  - 当对象只需要使用一次时，则可以使用匿名对象。

```java
public Student findById(int id) {
    name,gender,age
       
    return new Student(name,gender,age);
}
```

# 学习目标

- 独立编写代码完成一个对象作为另一个类的成员变量的练习
- 能够阐述继承的作用和好处
- 能够阐述继承的代码格式,独立编写一个继承的案例
- 独立编写代码通过set和get方法给对象设置属性
- 能够阐述java中继承的特点
- 能够阐述方法重写的概念及格式
- 能够阐述Override注解的作用及使用方法
- 能够描述继承后子父类内存图
- 能够阐述this和super访问普通成员的格式
- 能够阐述this和super访问构造方法的格式

# 今天内容

- 类与类之间的关系
  - 组合关系
  - 继承关系

## 1. 组合关系

- 在Java中类与类之间的关系有哪些？
  - 组合关系
  - 继承关系
  - 代理关系(后面的内容)


- 组合关系的概述
  - 在一个数据类型A中成员变量的数据类型是类型B时，此时A和B就是组合关系。


- 组合关系的案例
  - 人和宠物


- 代码演示

```java
/**
 组合关系的概述
	- 在一个数据类型A中成员变量的数据类型是类型B时，此时A和B就是组合关系。
 */
public class Person {
	// 姓名
	private String name;
	// 年龄
	private int age;
	// 宠物
	private Pet pet;
	public Person(String name, int age, Pet pet) {
		super();
		this.name = name;
		this.age = age;
		this.pet = pet;
	}
	public Person() {
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
	public Pet getPet() {
		return pet;
	}
	public void setPet(Pet pet) {
		this.pet = pet;
	}
	
	/**
	 * 显示个人信息和宠物信息
	 */
	public void showInfo() {
		// 人的信息
		System.out.println("我的名字：" + this.name  + ",我的年龄：" + age);
		// 宠物的信息
		System.out.println("我的宠物名称是："+ this.pet.getName() +"品种是："
		+this.pet.getKind()+"颜色是：" + this.pet.getColor());
	}
}

/**
 * 宠物类
 * @author pkxing
 *
 */
public class Pet {
	// 姓名
	private String name;
	// 品种
	private String kind;
	// 颜色
	private String color;
	public Pet(String name, String kind, String color) {
		super();
		this.name = name;
		this.kind = kind;
		this.color = color;
	}
	public Pet() {
		super();
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getKind() {
		return kind;
	}
	public void setKind(String kind) {
		this.kind = kind;
	}
	public String getColor() {
		return color;
	}
	public void setColor(String color) {
		this.color = color;
	}
}

/**
 * 组合关系测试类
 * @author pkxing
 *
 */
public class Demo01 {
	public static void main(String[] args) {
		// 创建Person对象
		Person p = new Person("张三",23,new Pet("旺财", "哈士奇", "绿色"));
		// 显示信息
		p.showInfo();
	}
}
```

![](image01.png)

## 2. 继承关系的引入

定义一个Teacher类(子类)，继承 Person类

 		// 成员变量：name,age

 		// 成员方法：eat(),sleep() 

定义一个Student类(子类) 继承 Person类

 		成员变量：score

 		成员方法：study()

定义一个Worker类(子类) 继承 Person类

 		// 成员变量：name,age

 		// 成员方法：eat(),sleep();	

定义一个Person类(父类或超类或基类)

 		成员变量：name,age

 		成员方法：eat(),sleep(),drink()

## 3. 继承的概述及特点

### 3.1 继承的概述

-  继承是类与类关系的一种。
- 继承是面向对象的三大特征之一。
- 从类与类关系的设计角度来看，子类必须是父类的一种才能使用继承。

### 3.2 继承的好处

-  提高了代码的复用性。
- 提高了代码的扩展性。
- 为多态提供前提。

###  3.3 继承的特点

- 子类拥有(除构造方法以外的)父类所有的成员变量和成员方法。
- 子类可以拥有自己特有的成员变量和成员方法
- 子类可以直接访问父类非private修饰的成员：成员变量和成员方法。
- 子类可以使用自己的方式来实现父类的方法。(方法重写)

 	* 构造方法不能被子类继承，但是子类可以间接调用父类的构造方法。	

## 4. 继承的格式

```java
class 子类类名 extends 父类类名{
    
}
```

- 代码演示

```java
/**
 * 人类：父类
 */
public class Person {
	// 成员变量
	public String name;
	public int age;
	
	// 成员方法
	public void eat() {
		System.out.println("吃个蛋炒饭");
	}
	public void sleep() {
		System.out.println("睡个毛线");
	}
}

/**
 * 老湿类：继承 Person 类
 */
public class Teacher extends Person{
	public double salary;
	public void teach() {
		System.out.println("好好教学");
	}
}

/**
 * 学生类：继承 Person 类
 */
public class Student extends Person {
	// 成员变量
	public double score;
	// 成员方法
	public void study() {
		System.out.println("好好学习，day day up");
	}
}

public class Demo01 {
	public static void main(String[] args) {
		// 创建老师对象
		Teacher t = new Teacher();
		// 访问成员变量
		t.name = "山哥";
		t.age = 30;
		t.salary = 300000;
		// 调用成员方法
		t.eat();
		t.sleep();
		t.teach();
		System.out.println(t.name+"=" + t.age);
		
		// 创建学生对象
		Student s = new Student();
		s.name = "山鸡";
		s.age = 40;
		// 调用成员方法
		s.eat();
		s.sleep();
		s.study();
		System.out.println(s.name+"=" + s.age);
	}
}
```

## 5. 继承的注意事项

- Java中类只支持单继承，不支持多继承。
- Java支持多层继承

```java
/**
 * 继承的注意事项
 	* 类支持单继承，不支持多继承
 	* 支持多层继承
 
  继承须知
  	* 所有的类都是Object类子类。
  	* 所有的类都间接或直接继承Object类(Object类祖宗类，超类)
 */
public class Demo02 {
	public static void main(String[] args) {
		// 创建Son对象
		Son s = new Son();
		s.test01();
		s.test02();
		s.test03();
	}
}

class Fu1 extends Object{
	public void test01() {
		System.out.println("fu1 test01");
	}
}

class Fu2 extends Fu1 {
	public void test02() {
		System.out.println("fu2 test02");
	}
}

class Son extends Fu2{
	public void test03() {
		System.out.println("fu3 test03");
	}
}
```

## 6. 通过set和get方法访问对象属性

- 在实际开发中，父类的成员变量一般都要求使用private 修饰
- 如果使用private修饰之后，子类就不能直接访问成员变量
- 那么此时父类应该要为private修饰的成员变量提供对应的setter&getter方法
- 子类就可以通过setter&getter方法来间接访问父类中的成员变量。

```java
/**
 * 通过set和get方法访问父类的成员变量
 */
public class Demo01 {
	public static void main(String[] args) {
		// 创建老师对象
		Teacher t = new Teacher();
		// 访问成员变量
		t.setName("山哥");
		t.setAge(30);
		t.setSalary(30000);
		// 调用成员方法
		t.eat();
		t.sleep();
		t.teach();
		System.out.println(t.getName()+"=" + t.getAge());
	}
}
```

## 7. 方法重写的概念和格式

### 7.1 方法重写的概念

- 在子类中，出现了和父类方法声明完全一样的方法(方法名相同，返回值类型相同，参数列表相同)，只是方法体不同，则称为方法重写。

### 7.2 方法重写的格式

-  与父类方法声明完全一致即可。

###  7.3 什么时候会使用方法重写？

- 在父类方法的功能满足不了子类需求时，子类可以通过方法重写来实现自己的功能。

### 7.4 代码演示

```java
/**
 手机类：父类
 	属性：价格和品牌
 	功能：发短信和打电话
 */
public class Phone {	
	// 成员变量
	private String brand;
	private double price;
	
	public String getBrand() {
		return brand;
	}
	public void setBrand(String brand) {
		this.brand = brand;
	}
	public double getPrice() {
		return price;
	}
	public void setPrice(double price) {
		this.price = price;
	}
	
	// 成员方法
	public void sendMessage() {
		System.out.println("发短信");
	}
	
	public void call() {
		System.out.println("打电话.....");
	}
}

/**
 * Iphone 继承 Phone
 * @author pkxing
 *
 */
public class IPhone extends Phone {
	public void call() {
		System.out.println("打电话");
		System.out.println("显示归属地");
		System.out.println("显示用户头像");
	}
}

/**
  方法重写的概念
 	 * 在子类中，出现了和父类方法声明完全一样的方法(方法名相同，返回值类型相同，参数列表相同)，只是方法体不同，则称为方法重写。
 	
  方法重写的格式
 	* 与父类方法声明完全一致即可。
 	
  什么时候会使用方法重写？
 	* 在父类方法的功能满足不了子类需求时，子类可以通过方法重写来实现自己的功能。
 
 */
public class Demo01 {
	public static void main(String[] args) {
		// 创建IPhone手机对象
		IPhone ip = new IPhone();
		ip.setBrand("苹果手机");
		ip.setPrice(8000);
		
		// 发短信和打电话
		ip.sendMessage();
		ip.call();
	}
}
```

## 8. 方法重写的注意事项

- 方法重写后，调用方法不再调用父类的方法，而是调用子类重写后的方法。


- 如果父类的方法使用private修饰，则子类不能重写该方法，即使子类有方法声明一样的方法，都不属于重写，属于重新了一个同名的方法。


-   子类重写方法时方法的修饰权限符要大于等于父类方法使用的权限修饰符
  - public > protected > 默认 > private 	

## 9. Override注解介绍

- Override的作用
  - 用来修饰方法声明的，告诉编译器该方法是重写父类的方法，如果父类没有该方法，则编译失败。
  - 只要方法声明和父类方法声明一样，有没有该注解修饰都属于方法重写。
- 代码演示

```java
/**
 * Iphone 继承 Phone
 * @author pkxing
 */
public class IPhone extends Phone {
	 @Override // 注解
	 public void call() {
		super.call();
		System.out.println("显示归属地");
		System.out.println("显示用户头像");
	}
}
```

## 10. this和super关键字访问普通成员(非private)

### 10.1 super关键字访问普通成员：成员变量和成员方法

-  super.成员变量名; 
  - 直接去父类中，父类没有，再去父类...直到Object类，如果都没有找到，则编译失败。


-  super.成员方法名(参数)
  - 直接去父类中，父类没有，再去父类...直到Object类，如果都没有找到，则编译失败。

### 10.2 this关键字访问普通成员：成员变量和成员方法

-  this.成员变量名; 
  - 先在本类找成员变量，本类没有，找父类，直到找Object类，如果都没有找到，则编译失败。


-  this.成员方法(参数);
  - 先在本类找成员变量，本类没有，找父类，直到找Object类，如果都没有找到，则编译失败。


-  this和super关键字方法普通成员的查找原则
  - this：本类 > 父类 > ... > Object
  - super：父类 > ... > Object

## 11. super和this访问构造方法

### 11.1 this关键字调用构造方法

-  this(参数列表); 调用本类有参数构造方法


-  this(); 调用本类无参数构造方法 	

### 11.2 this关键字调用构造方法注意事项

- 必须使用在构造方法中。


-  必须是构造方法中的第一行有效语句。

###  11.3 super关键字调用构造方法

-  super(参数列表); 调用父类有参数构造方法


-  super(); 调用父类无参数构造方法

###  11.4 super关键字调用构造方法注意事项

- 必须使用在构造方法中。


-  必须是构造方法中的第一行有效语句。


-  this和super不能同时使用在构造方法中调用构造方法。

### 11.5 须知

-  每一个子类的构造方法中，默认第一行语句都是调用父类默认构造方法


-  如果在子类构造方法中通过super调用了父类的构造方法，则就不会默认调用父类的无参数构造方法了。

### 11.5 代码演示

```java

/**
 * 父类
 */
public class Person {
	private String name;
	private int age;
	
	public Person() {
		/*this.name = "张三";
		this.age = 23;*/
		
		// this("张三",23);
		
		// this(); 递归
		System.out.println("无参数构造");
	}
	
	public Person(String name, int age) {
		System.out.println("进来了吗");
		this.name = name;
		this.age = age;
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
	
	public void show() {
		System.out.println(this.name + "=" + this.age);
	}
}

/**
 * 子类
 * @author pkxing
 *
 */
public class Student extends Person{
	private double score;

	public Student() {
		
	}

	public Student(String name, int age) {
		this(name,age,0.0);
	}

	public Student(String name, int age, double score) {
		/*this.setName(name);
		this.setAge(age);
		*/
		super(name,age);
		this.score = score;
	}

	public double getScore() {
		return score;
	}

	public void setScore(double score) {
		this.score = score;
	}
	
	@Override
	public void show() {
		super.show();
		
		System.out.println("score = " + score);
	}
}

public class Demo01 {
	public static void main(String[] args) {
		// 创建Student对象
		Student s = new Student("LILY", 18, 100);
		// 显示信息
		s.show();
	}
	public static void test01() {
		// 创建Person对象
		// Person p = new Person("jack",18);
		Person p = new Person();
		/*
		// 设置姓名和年龄
		p.setName("rose");
		p.setAge(20);
		*/
		// 显示信息
		p.show();
	}
}
```

## 13. 继承后子父类内存分析

![](image02.png)

## 14. 继承练习
### 14.1 需求描述

```java
- 员工类
  - 员工姓名,员工薪水,员工工作方法
- 经理类
  - 除了员工属性还多加了奖金
  - 工作方法为打印"管理酒店"
- 服务员类
  - 属性和员工的属性相同
    - 工作方法为打印"上菜与结账"
- 厨师类
  - 属性和员工的属性相同
  - 工作方法为打印"做饭"
- 测试
  - 共有1个经理,2个服务员,1个厨师,(所有人的属性值任意定义)
  - 让所有的员工工作
  - 并求所有人的总收入是多少?
```

- 代码实现

```java
/**
 * 员工类：父类
 * @author pkxing
 *
 */
public class Employee {
	// 成员变量
	private String name;
	private double salary;
	
	public Employee() {
	}
	
	public Employee(String name, double salary) {
		this.name = name;
		this.salary = salary;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public double getSalary() {
		return salary;
	}

	public void setSalary(double salary) {
		this.salary = salary;
	}

	// 工作方法
	public void work() {
		System.out.println("工作");
	}
}


/**
 * 厨师类
 * @author pkxing
 *
 */
public class Cook extends Employee {
	@Override
	public void work() {
		System.out.println("做饭");
	}

	public Cook() {
		super();
	}

	public Cook(String name, double salary) {
		super(name, salary);
		// TODO Auto-generated constructor stub
	}
	
	
}


/**
 * 经理类
 * @author pkxing
 *
 */
public class Manager extends Employee {
	// 奖金
	private double bouns;
	
	public Manager() {
		super();
	}

	public Manager(String name, double salary) {
		super(name, salary);
	}
		
	public Manager(String name, double salary, double bouns) {
		super(name, salary);
		this.bouns = bouns;
	}

	public double getBouns() {
		return bouns;
	}

	public void setBouns(double bouns) {
		this.bouns = bouns;
	}
	
	@Override
	public void work() {
		System.out.println("管理酒店");
	}
	
}

/**
 * 服务员
 * @author pkxing
 *
 */
public class Waiter extends Employee {	
	@Override
	public void work() {
		System.out.println("上菜与结账");
	}

	public Waiter() {
		super();
	}

	public Waiter(String name, double salary) {
		super(name, salary);
	}
	
	
}


public class Demo01 {
	public static void main(String[] args) {
		// 创建经理对象
		Manager m = new Manager("山哥",15000,20000);
		
		// 创建两个服务员对象
		Waiter w1 = new Waiter("小泽", 2500);
		Waiter w2 = new Waiter("小苍", 3500);
		
		// 创建厨师
		Cook c = new Cook("小野", 8000);
		
		// 让所有的员工工作
		m.work();
		w1.work();
		w2.work();
		c.work();
		
		// 并求所有人的总收入是多少
		double salary = m.getSalary() + w1.getSalary() + w2.getSalary() + c.getSalary() + m.getBouns();
		System.out.println(salary);
	}
}
```


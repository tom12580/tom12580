# 知识点回顾

- 抽象类和抽象方法

  - 抽象方法的格式

  ```java
  修饰符 abstract 返回值类型 方法名(参数列表);
  ```

  - 抽象类的格式

  ```java
  修饰符 abstract class 类名{
      // 定义抽象方法
      // 定义非抽象方法
  }
  ```

  - 不能直接创建抽象类的对象，只能使用子类。
  - 子类继承抽象类之后，需要重写抽象类的中所有抽象方法。否则该类必须声明为抽象类。

- 接口

  - 接口是功能集合，定义了功能具备的方法，如果实现这些方法由实现类(子类)通过方法重写完成。也是一种数据类型，比抽象类更加抽象的类
  - 定义格式

  ```Java
  public interface 接口名{
      // 常量
      public static final String name = "张三";
      // 抽象方法
      public abstract void play();
      // 默认方法：JDK1.8
      default void show(){
          
      }
  }
  ```

  - 使用格式

  ```java
  class 类 implements 接口名1,接口2...接口n{
      // 重写所有实现的接口中的所有的抽象方法，否则该类必须声明为抽象类。
  }
  ```

- 多态概念

  - 同种事物表现出不同的形态。

- 多态的前提

  - 必须有子父类关系或类实现接口关系
  - 必须有方法重写
  - 必须有父类引用指向子类对象

- 多态的格式

  ```java
  class Animal{
      public void eat(){
          
      }
  }

  class Dog extends Animal{
       public void eat(){
          
      }
  }
  class Cat implements Play{
      public void catchMouse(){
          
      }
      public void playing(){
          sop.....
      }
  }

  interface Play{
      public void playing();
  }

  class Test{
      public static void main(String[] args) {
          Animal d = new Dog();
          
          Play c = new Cat();
          c.playing();
          
          // 向下转型
          Cat cc = (Cat)c;
          cc.catchMouse();
      }
  }
  ```

# 学习目标

* 能够阐述static修饰符的作用及使用场景
* 能够阐述静态的访问方式及注意事项
* 能够阐述final修饰类、方法、变量有什么特点
* 能够阐述包的概念及定义包的格式
* 能够阐述包的全名访问格式
* 能够使用import导入其他包中的类
* 能够阐述四种权限修饰符在同包的访问情况，跨包的访问情况，不同包子父类的访问情况
* 能够阐述内部类的概念及内部类的分类
* 能够阐述成员内部类定义的格式及成员内部类的使用方式
* 能够阐述局部内部类定义的格式及局部内部类的使用方式
* 独立编码创建匿名内部类并使用匿名内部类

# 今天内容

- static关键字
- final关键字
- 包的概念和使用
- 权限修饰符
- 内部类

## 1. static关键字概述

### 1.1 static修饰成员变量

```java
static关键字的概述
 	* static是一个修饰符。
 	* static一般用来修饰成员变量，成员方法以及代码块。
 	* static修饰的成员具有特点：该成员不再属于对象，而属于类，但是可以被该类的所有对象共享。
 	
成员变量的分类
 	静态成员变量：也叫类变量， 有static修饰的
 	非静态成员变量：也叫实例变量或对象变量，没有static修饰的

静态成员变量和非静态成员变量的区别
 	语法的区别
 		静态成员变量：有static修饰的
 		非静态成员变量：没有static修饰的
 	访问方式区别
 		静态成员变量：可以通过对象名方法，也可以通过类名访问(强烈推荐使用)
 		非静态成员变量：只能通过对象名访问。
 	生命周期区别
 		静态成员变量：跟随类的加载而创建并初始化，跟随类的销毁而销毁。
 		非静态成员变量：跟随对象的创建而创建并初始化，跟随对象的销毁而销毁。
 	数量的区别
 		静态成员变量：在内存中只有一份，所有类的对象共用一个。
 		非静态成员变量：每次创建对象都会为成员变量分配内存并初始化，每一个对象都有自己的一份成员变量，互不干预。
 
什么时候使用static修饰成员变量？
	当该成员变量的值在该类的所有对象中都相同时，则应该将该成员变量定义为静态成员变量。
```

- 测试类

```java
/**
 * 学生类
 */
public class Student {
	// 姓名
	public String name;
	// 年龄
	private int age;
	// 学校名称
	public static String scName = "传智播客";
	
	public Student() {
		super();
	}
	public Student(String name, int age, String scName) {
		super();
		this.name = name;
		this.age = age;
		this.scName = scName;
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
	public String getScName() {
		return scName;
	}
	public void setScName(String scName) {
		this.scName = scName;
	}
	
	// 显示学生信息
	public void show() {
		System.out.println("姓名：" + name + ",年龄：" + age + ",学校名称：" + this.scName);
	}
}

public class StaticDemo01 {
	public static void main(String[] args) {
		System.out.println(Student.scName);
		// 创建学生对象
		Student s1 = new Student("张三", 23, "传智播客");
		Student s2 = new Student("李四", 24, "传智播客");
		// 显示学生信息
		s1.show();
		s2.show();
		
		
		// 通过s1修改scName的值
		// s1.setScName("黑马程序员");
		Student.scName = "黑马程序猿";
		// s1.setName("老王");
		s1.name = "老王";
		// Student.name = "";
		
		// 显示学生信息
		s1.show();
		s2.show();
	}
}
```

- 静态变量加载内存分析图

![](image01.png)

### 1.2 static修饰成员方法

```java
成员方法的分类
	* 静态成员方法：类方法
	* 非静态成员方法：实例方法
	
静态成员方法和非静态成员方法的区别
	* 静态成员方法：可以使用对象调用，也可以使用类名调用(强烈推荐使用类名调用)
	* 非静态成员方法：只能通过对象名调用。
	
static修饰方法的注意事项
	* 静态方法中不能使用this和super关键字。
	* 静态方法中不能访问非静态的成员：非静态成员变量和非静态方法。
	* 非静态方法中既可以访问静态成员也可以访问非静态成员。
	
什么时候使用static修饰成员方法？
	* 当成员方法中不需要访问到任何的非静态成员时，可以考虑将该方法定义为静态方法。
	* 在工具类中定义的方法一般都是静态方法。
		* Math,Arrays,Collections
```

- 数组工具类

```java
/**
 * 数组工具类
 */
public class ArrayTools {
	// 求数组最大值
	public static int getMax(int[] arr) {
		int max = arr[0];
		for (int i = 1; i < arr.length; i++) {
			if(max < arr[i]) {
				max = arr[i];
			}
		}
		return max;
	}
}
```

- 测试类

```java
public static void main(String[] args) {
		// 定义一个数组
		int[] arr = {12,45,43,12314,3242};
		int max = ArrayTools.getMax(arr);
		System.out.println(max);
		
		/*// 创建工具类对象
		ArrayTools tool = new ArrayTools();
		// 获得数组最大值
		int max = tool.getMax(arr);
		
		System.out.println(max);*/
}
```

## 2. final关键字概述

### 2.1 final关键字概述

- final也是一个修饰符。
- final一般用来修饰基本类变量，引用类型变量，方法，类等成员
- final修饰基本数据类型的变量：该变量就是常量了，其值不能再次赋值。
- final修饰方法：不能被子类重写了。
- final修饰类：该类不能被继承了。
- final修饰引用类型的变量：该引用变量就不能再指向其他对象。
  - final修饰引用类型的变量，指其所引用的对象不能改变，即该变量引用的地址值不能改变。对象成员变量的值是可以修改的。

### 2.2 代码演示

```java
package com.itheima._02final关键字;
/**
 * Final关键字使用
 */
final class Circle{
	// 半径
	private double r;
	// 圆周率
	// 常量命名规范：所有字母大小，如果多个单词组成，多个单词之间使用下划线分割
	public static final double PI = 3.1415926;
	private static final double MAX_VALUE = 100;
	
	public Circle(double r) {
		this.r = r;
	}
	public Circle() {
		super();
	}
	
	public double getR() {
		return r;
	}
	public void setR(double r) {
		this.r = r;
	}
	
	/**
	 * 计算面积方法
	 * final修饰的方法：不能再被子类重写。
	 */
	public final double getArea(){
		return PI * r * r;
	}
	
	/**
	 * 计算周长
	 */
	public double getZc() {
		return 2 * PI * r;
	}
}

/**
 * Final修饰的类不能被继承了
 */
/*class BigCircle extends Circle{
	@Override
	public double getZc() {
		return 100;
	}
}*/

public class FinalDemo01 {
	public static void main(String[] args) {
		/*// 创建圆对象
		Circle c = new Circle(10);
		// 修改圆周率的值
		// c.setPI(1000);
		
		System.out.println(c.getArea());
		System.out.println(c.getZc());*/
		
		// final修饰引用类型的变量：该引用变量就不能再指向其他对象。
		final Circle c1 = new Circle(5); // 0x1111
		// 该引用变量就不能再指向其他对象。
		// c1 = new Circle(20);
		test01(c1);
		System.out.println(c1.getArea()); 
	}
	
	public static void test01(final Circle c) { // c = 0x1111
		System.out.println(c.getArea()); 
		// c = new Circle(10); // 0x2222
		c.setR(10);

		System.out.println(c.getArea()); 
	}
}
```

## 3. 包的概述和定义格式

- 包的概念

  - 包在文件系统中就是一个文件夹。

- 包的作用

  - 解决类命名冲突的问题。
  - 将功能相同或相似的类以及接口组织在同一个文件夹中，方便类的管理和查找。

- 包的定义格式

  ```Java
  package 包名1.包名2...;
  包名的命名规则：
  	* 包名的英文字母全部小写。
  	* 一般公司域名单词倒着写命名
  		* 比如：www.itcast.cn,包名：cn.itcast.功能名称
  		* www.itheima.com,包名：com.itheima.功能名称
  		* www.baidu.com,  包名：com.baidu.
  ```

- 包的注意事项

  - 定义包的语句必须是类中的第一行有效语句。

## 4. 类的访问方式

```java
类的访问方式
  	* 不需要导包的情况
  		* 当被使用的类在java.lang包。
  		* 当被使用的类和当前的类是在同一个包下。 	
  			
 	* 使用类全名访问：包名.包名...类名
 	* 导包访问
 		* 格式1：import 包名.包名.类名; 导入一个类。
 		* 格式2：import 包名.包名.*;   导入指定包下的所有的类，不包括子包下的类。
 			
导包的注意事项
 	* 如果有两个包下有相同的类名，只能有一个被导包，另一个包下的类只能通过类全名访问。
```

- 代码演示

```java
import com.itheima._01static关键字.Student;
import java.util.*;
public class Demo01 {
	public static void main(String[] args) {
		// 创建集合对象
		ArrayList<Integer> list = new ArrayList<>();
		list.add(111);
		
		// 使用类全名访问
		// com.itheima._01static关键字.Student s = new com.itheima._01static关键字.Student();
		// s.eat();
		Student s = new Student();
		s.eat();
		
		// 创建本包学生对象
		com.itheima._03类的访问方式.Student stu = new com.itheima._03类的访问方式.Student();
		stu.sleep();
	}
}
```

## 5. 四种访问权限修饰符

- 四种权限修饰符的特点
  - public：任意包下的任意类都可以访问。
  - protected：任意包下的子类都可以访问。
  - 默认(包权限)：同包下的任意类都可以访问。
  - private：只能在本类中使用
- 四种权限修饰符图

![](访问权限修饰符图.png)

- 小结
  - 只要是成员变量使用private修饰
  - 只要是成员方法使用public修饰

## 6. 内部类的概述

### 6.1 内部类的概念

- 一个类的定义在另一类中，则该类称为内部类。
  - 比如：在A类中定义了B类，B类称为内部类，A称为外部类。
- 内部类可以访问外部类的所有成员，包括private修饰的。
- 内部类的字节码文件：外部类名$内部类名.class

### 6.2 内部类分类 

- 成员内部类(了解)：定义在成员位置，和成员变量是同级别的。
- 局部内部类(了解)：定义在外部类的方法中，和局部变量是同级别的。
- 匿名内部类(重点)

## 7. 成员内部类
### 7.1 定义格式

```java
/**
 * 外部类
 */
public class Outer {
	// 成员变量
	
	// 成员内部类
	class Inner{
		// 成员变量
		
		// 构造方法
		
		// 成员方法
		
		// 普通成员方法
	}
}
```

### 7.2 访问方式

- 方式1：在外部类中定义一个方法，在该方法中创建内部类对象，然后在测试类中创建外部类对象，调用外部类的方法。

- 方式2：直接创建外部类Outer的内部类Inner的对象。

  ```java
  格式：外部类名.内部类名 变量名 = new 外部类名().new 内部类名();
  ```

### 7.3 使用场景

- 当一个类只被另一个类使用时，则可以考虑将该类定义为另一个类的成员内部类。


-  当描述事物A时，发现事物A中还包含事物B，而且事物B需要使用到事物A的一些数据，此时就应该将事物B定义为事物A的成员内部类。

```java
class 人{
 				血液;
 				氧气;
 				class 心脏{
 					血液;
 					氧气;
 					跳动();
 				}
 			} 
```

### 7.4 注意事项

- 如果成员内部类使用private修饰，则不能在外部直接创建内部类的对象。
- 成员内部类不能定义静态成员，如果要定义，则该内部类也必须使用static修饰。
- 如果内部类和外部类出现同名的成员变量时，在内部类中访问变量时访问的是内部类定义的。如果要访问外部类的同名的成员变量时，则要使用以下语法：

```Java
外部类名.this.成员变量名;
外部类名.this.成员方法名(参数列表);
```

## 8. 局部内部类
### 8.1 定义格式

```java
/**
 * 外部类
 */
public class Outer02 {
	/**
	 * 外部类的test方法
	 */
	public void test() {
		// 定义局部内部类
		class Inner{
			// 成员变量
			// 构造方法
			// 成员方法
		}
	}
}
```

### 8.2 访问方式

- 只能在当前方法中创建该局部内部类的对象，然后调用方法。

```java
/**
 * 外部类
 */
public class Outer02 {
	// 成员变量
	private String name = "jack";
	private int age;
	
	/**
	 * 外部类的test方法
	 */
	public void test() {
		// 局部变量
		 int age = 100;
		// 定义局部内部类
		class Inner{
			// 局部内部类中不能定义静态变量
			// private static String gender = "男";
			private String name = "rose";
			// 普通成员方法
			public void show() {
				// age = 200;
				System.out.println(age + ":内部类的show方法:" + this.name);
			}
		}
		// 创建内部类对象
		Inner inner = new Inner();
		inner.show();
		System.out.println("age = " + age); // 100
	}
}
/**
 * 局部内部类的基本使用
 */
public class InnerDemo02 {
	public static void main(String[] args) {
		Outer02 outer = new Outer02();
		outer.test();
	}
}
```

### 8.3 注意事项

- 局部内部类不能使用权限修饰符：public，protected，private
- 在JDK1.8之前，局部内部类中访问局部变量时，该局部变量需要使用final修饰。
- 在JDK1.8(包含1.8)之后，局部内部类中访问局部变量时，该局部变量不要求使用final修饰。
  但是也不能对局部变量进行修改操作。

## 9. 匿名内部类
### 9.1 匿名内部类概述

- 没有创建类的语法，直接创建已知类的子类对象或已知接口的实现类对象。

### 9.2 匿名内部类的前提条件

- 必须有一个类或接口。

### 9.3 匿名内部类的格式

```java
new 类名或接口名(){
   // 重写方法  
};
```

- 代码演示

```java
/**
 * 匿名内部类
 * @author pkxing
 *
 */
/**
 * 动物类
 */
abstract class Animal{
	public abstract void eat();
}

class Dog extends Animal{

	@Override
	public void eat() {
		
	}
}

/**
 * 接口
 * @author pkxing
 *
 */
interface Play{
	void playing();
}
public class InnerDemo03 {
	public static void main(String[] args) {
		// 使用匿名内部类：直接创建Animal的子类对象
		/**
             Animal a = new Animal() {
                @Override
                public void eat() {
                    System.out.println("吃饭");
                    sleep();
                }
            };
            ### 上一行语句等价于下面两行语句
            class Xxx extends Animal{
                @Override
                public void eat() {
                    System.out.println("睡觉");
                }
            }
            Animal a = new Xxx();
		 */
        
         /*
         以下代码做了两件事情：
         	 1：定义了一个子类继承Animal，重写方法。不知道类名
		    2：创建子类对象
         */
		Animal a = new Animal() {
			public String name = "猪";
			public void sleep() {
				System.out.println("睡觉");
			}
			@Override
			public void eat() {
				System.out.println("吃饭");
				sleep();
			}
		};
		// 调用eat方法
		a.eat();
		
		// 直接创建接口的实现类对象
		/*
		class ooo implements Play{
			@Override
			public void playing() {
				System.out.println("玩耍");
			}
		}
		new ooo();
		*/
        /*
         以下代码做了两件事情：
         	  1. 定义了一个类实现了Play接口
	          2. 创建实现类对象
         */
		Play p =  new Play() {
			@Override
			public void playing() {
				System.out.println("玩耍");
            }	
		};
		p.playing();
	}
}
```

### 9.4 常见问题

* 匿名内部类能定义构造方法吗？
  * 不能定义构造方法，因为不知道类名是什么。
* 匿名内部类能定义新的成员方法和成员变量吗？
  * 可以定义，但没有意义，因为没有办法在外部调用到新定义的成员。
# 知识点回顾

- 组合关系

  - 当一个类型A的成员变量的数据类型是类型B时，A和B就是组合关系。

- 继承的格式

  ```java
  class 子类 extends 父类{
      
  }
  ```


- 继承的特点
  - 子类拥有除了构造方法以外的所有成员：成员变量和成员方法。
  - 子类可以直接访问父类非private 修饰的成员变量和成员方法
  - 子类可以拥有自己的特有的成员变量和成员方法。
  - 子类可以使用自己的方式实现父类的方法。
  - 构造方法不能被子类继承，但子类可以间接访问父类的构造方法。


- 方法重写的概念
  - 子类中出现了和父类方法声明完全一样的方法则称为方法重写。
    - 返回值类型要一样
    - 参数列表要一样
    - 方法名要一样
    - 方法体不一样。
- 方法重写的注意事项
  - 父类private修饰的方法不能被子类重写，即使子类有同名的方法也不属于重写，属于重写定义了一个新方法
  - 子类重写方法时方法的权限修饰符要大于等于父类方法的权限修饰符
    - public > protected > 默认 > private
  - 子类重写方法后调用方法时是调用子类重写后的方法。
- Override注解的作用
  - 用来修饰方法声明。用来表明该方法是重写父类的方法，如果父类没有该方法，则编译失败。
- this和super关键字访问成员变量和成员方法
  - this.成员变量名;    本类中找 > 父类中找 ….> Object 类
  - this.成员方法名(参数);  本类中找 > 父类中找 ….> Object 类
  - super.成员变量名;      父类中找 ….> Object 类
  - super.成员方法名(参数);    父类中找 ….> Object 类


- this和super调用构造方法
  - this(参数);  调用本类的构造方法。
  - super(参数); 调用父类的构造方法。
    -  必须在构造方法中使用。
    - 必须是构造方法中的第一行有效语句。
    - this和super用来调用构造方法不能同时出现。

# 学习目标

* 能够独立定义一个抽象方法
* 能够独立定义一个抽象类并在抽象类中定义抽象方法,实现该抽象类中抽象方法
* 能够利用子类来使用抽象父类中的构造方法
* 能够自定义一个接口,并实现该接口
* 能够阐述接口中成员的默认修饰符
* 能够阐述接口的注意事项
* 能够阐述接口和抽象类的区别
* 能够阐述什么是多态的向上转型和向下转型
* 能够使用instanceof关键字判断对象的类型
* 能够阐述多态的优点和使用场景

# 今天内容

- 抽象类和抽象方法
- 接口
- 多态

# 第一章 抽象类和抽象方法

## 1. 抽象类与抽象方法的引入

```java
/**
 * 圆形类：继承 Shape
 * @author pkxing
 */
class Circle extends Shape{
	// 半径
	private double radius;
	
	public double getRadius() {
		return radius;
	}

	public void setRadius(double radius) {
		this.radius = radius;
	}
	

	public Circle(double radius) {
		super();
		this.radius = radius;
	}

	public Circle() {
		super();
	}

	@Override
	public void getArea() {
		System.out.println(3.14 * radius * radius);
	}

	@Override
	public double getZc() {
		return 2 * 3.14 * radius;
	}
	
}

/**
 * 定义一个图形类：Shape
 */
abstract class Shape{
	// 计算面积的方法
	public abstract void getArea();
	// 计算周长的方法
	public abstract double getZc();
}
/**
 * 狗类：继承 动物类
 */
class Dog extends Animal{
	public void cry() {
		System.out.println("汪汪...");
	}
}
class Cat extends Animal{
	@Override
	public void cry() {
		
	}
}
/**
 *	动物类：Animal
 */
abstract class Animal{
	// 叫的方法
	 public abstract void cry();
	 // 吃的方法
	 public void eat() {
		 System.out.println("吃");
	 }
}


public class Demo01 {
	public static void main(String[] args) {
		// 创建动物:不能直接创建Animal对象，因为Animal是抽象类
		// Animal a = new Animal();
		
		// 创建狗对象
		Dog d = new Dog();
		d.cry();
		
		// 创建圆形对象
		Circle c = new Circle(10);
		c.getArea();
		System.out.println(c.getZc());
	}
}

```

## 2. 抽象类和抽象方法的概念
### 2.1 抽象方法的概念

- 使用abstract关键字修饰的方法就是抽象方法

### 2.2 抽象方法的定义格式

```java
修饰符 abstract 返回值类 方法名(参数列表);
```

### 2.3 抽象类的概念

- 被abstract修饰的类就是抽象类。
- 有抽象方法的类是抽象类。

### 2.4 抽象类的定义格式

```java
abstract class 类名{
	// 抽象方法....
}
```

### 2.5 抽象类的作用

-  用来描述一种数据类型应该具备的基本特征(成员变量)和行为(方法)，如何实现这些方法由子类通过方法重写完成。

## 3. 抽象类的特点

- 抽象方法和抽象类都需要使用关键字：abstract 修饰


- 抽象类不能创建对象，因为调用抽象方法没有任何意义。
- 子类继承抽象类之后，要抽象类中的所有抽象方法，否则要将子类也定义为抽象类。

## 4. 抽象类的常见问题
### 4.1 抽象类不能创建对象，那么抽象类中是否有构造方法？如果有？构造方法有什么意义？ 

- 有，子类可以通过super调用父类的构造方法，给父类的成员变量赋值，赋值完之后子类就可以使用父类继承的成员变量。

### 4.2 抽象类一定是个父类吗？

- 一定是个父类，因为抽象类不能创建对象。

### 4.3 抽象类中可以不定义抽象方法吗？

- 可以。

### 4.4 抽象类中可以定义普通方法吗？(有方法体的方法)

- 可以

### 4.5 抽象关键字不可以和哪些关键共存？ 

- 不能和private关键字共存。因为private 修饰的方法不能被子类重写，而抽象方法又要求子类要重写。

# 第二章 接口

## 1. 接口的引入
## 2. 接口的概念

- 接口也是一种数据类型。它是比抽象类更加抽象的'类'


- 接口是功能(方法)的集合，如何实现这些功能由实现类(子类)通过方法重写实现。
- 接口中的方法都是抽象方法。

## 3. 接口的定义和使用格式
### 3.1 定义格式

```java
interface 接口名{
		// 抽象方法
}
```

### 3.2 使用格式

```java
class 类 implements 接口名 {
		// 重写抽象方法
}
```

### 3.3 示例代码

```java
public class Dog implements Perform{
	@Override
	public void show() {
		System.out.println("🐶胸口碎大石");
	}
}
```

## 4. 接口的特点和注意事项
### 4.1 接口特点

- 在JDK1.8之前，接口中所有的方法都是抽象方法，在JDK1.8后，接口中的方法可以有默认实现(有方法体)
- 接口没有构造方法
- 接口中定义成员变量是常量。
- 接口不能创建对象
- 接口可以继承接口，而且可以多继承。
- 接口中的抽象方法有默认的修饰符：public abstract
- 接口中的常量有默认的修饰符：public static final

### 4.2 注意事项

- Java支持类在继承一个类的同时实现1个或多个接口。
- 接口与父类的功能可以重复，均代表要具备某种功能，并不冲突。
- 当一个类实现了接口时，必须实现其所有的方法，否则这个类必须定义为一个抽象类。

## 5. 接口和抽象类的区别

- 相同点
  - 都不能创建对象
  - 子类或实现类都必须重写抽象方法


- 不同点
  - 接口可以接口且可以多继承， 类只能单继承，不能多继承。
  - 接口中的成员变量是常量了，抽象类可以有普通成员变量。
  - 抽象类中可以有构造方法，接口中没有构造方法
  - 接口中的抽象方法有默认的修饰符：public abstract，抽象类的抽象方法没有默认的修饰符。
  - 接口中的常量有默认的修饰符：public static final，抽象类的成员变量没有默认修饰符。
  - 抽象类中可以有非抽象方法，接口在JDK.18(包括1.8)之后才可以有非抽象方法。

## 6. 接口和抽象类的选择

- 先考虑该功能是否是这种数据类型或子类类型共有的功能
  - 如果是，则抽取到父类中，然后考虑功能是否会被所有的子类重写，如果是，则将该方法定义为抽象方法，那么该类就定义抽象类。 
  - 如果不是，则抽取到接口中，需要该功能的类型来实现接口，重写方法即可。

# 第三章 多态

## 1. 多态的概述

### 1.1 多态的概述

- 多态是面向对象的第三大特征。


- 同一种事物表现出来的多种形态则称为多态。
  - 比如：张三 ==> 学生   张三 ==> 路人甲   张三 ==> 儿子 
  - 比如：狗 ==> 动物   狗 ==> 宠物   

### 1.2 多态的前提

- 必须有子父类关系或类实现接口的关系。

- 必须有方法重写

- 必须有父类引用指向子类对象

  ```java
  abstract class Animal{
      public abstract void eat();
  }

  // 子父类关系
  class Dog extends Animal{
      // 方法重写
      public void eat(){
          syso("狗吃骨头")
      }
  }

  class Demo01{
      public static void main(String[] args){
          // 创建狗对象
          Animal d = new Dog();
          d.eat();
      }
  }
  ```

### 1.3 多态的格式

```java
父类引用 变量名 = new 子类();
或
接口类型 变量名 = new 接口实现类();
```

## 2. 多态案例

```java
/**
 * 多态：父类引用指向子类对象
 */
public class Demo01 {
	public static void main(String[] args) {
		// 创建狗对象：子类引用指向子类对象
		// Dog d = new Dog();
		Animal d = new Dog();
		// 吃饭和睡觉
		d.eat();
		d.sleep();  
	}
}
```

## 3. 多态的注意细节

-  多态情况下,子父类出现同名的成员变量时，访问的是父类的成员变量


- 多态情况下,子父类出现同名的成员方法时，访问的是子类的成员方法

  编译的特性

-  成员变量，编译看左边，运行也看左边。


-  成员方法，编译看左边，运行看右边。

```java
/**
 * 多态：父类引用指向子类对

  3. 多态的注意细节
  	* 多态情况下,子父类出现同名的成员变量时，访问的是父类的成员变量
  	* 多态情况下,子父类出现同名的成员方法时，访问的是子类的成员方法
  	
  	编译的特性
  		* 成员变量，编译看左边，运行也看左边。
  		* 成员方法，编译看左边，运行看右边。
 */
public class Demo01 {
	public static void main(String[] args) {
		// 创建狗对象：子类引用指向子类对象
		// Dog d = new Dog();
		Animal d = new Dog();
		System.out.println(d.name); // 访问的是父类的成员变量name
		// 吃饭和睡觉
		d.eat();
		d.sleep();  // 调用的是子类重写后的方法
		
		// d.show();
	}
}
```

## 4. 多态的好处(优点)与弊端(缺点)

- 优点
  -  提高代码的复用性。
  - 提高代码的扩展性。
  - 提高代码的维护性。


-  缺点
  - 不能访问子类特有的成员：成员变量和成员方法。

## 5. 多态的使用场景

- 作为形式参数
  - 多态用于形式参数类型的时候可以接收更多类型的对象
- 作为返回值类型
  - 多态用于返回值类型的时候可以返回更多类型的对象。

## 6. 多态的转型	

转型分类

- 向上转型

  - 当把子类对象赋值给一个父类引用时，便是向上转型，多态本身就是向上转型的过程。

  - 使用格式
    父类类型  变量名 = new 子类类型();

    ```java
    如：Person p = new Student();
    ```

- 向下转型

  - 一个已经向上转型的子类对象可以使用强制类型转换的格式，将父类引用转为子类引用，这个过程是向下转型。如果是直接创建父类对象，是无法向下转型的。
    使用格式		

    ```java
    子类类型 变量名 = (子类类型) 父类类型的变量;
    如:Student stu = (Student) p;  
    //变量p 实际上指向Student对象
    ```

## 7. instanceof关键字

- instanceof的作用
  - 用来判断某个对象是否属于某种数据类型。如学生的对象属于学生类，学生的对象也属于人类。
- 使用格式
  - boolean  b  = 对象名  instanceof  类名或接口名;
- 注意事项
  - 如果instanceof右边是类名，则要求左边的对象名和类名必须存在子父类关系。

```java

/**
	多态的转型
		向上转型：父类引用指向子类对象的过程就是向上转型的结果。
			Animal a = new Dog();
			
		向下转型: 将已经向上转型引用强制转换为子类引用的过程。
			long a = 100;
			int b = (int)a;
			转换格式： 子类类名 变量名 = (子类类名)父类引用变量。
					 或 
					 接口名 变量名 = (接口名)父类引用变量
					 
			比如：Dog d = (Dog)a;
			
	instanceof关键字 
		作用：用来判断父类引用到底指向哪个子类类型的对象。
		格式：boolean flag = 父类引用 instanceof 子类类名或接口名;
		注意事项：instanceof关键字右边如果给定的是类名，则要求类型和父类引用变量的类型要有继承关系。
 */
public class Demo04 {
	public static void main(String[] args) {
		// 创建一只狗对象
		Dog d = new Dog(); 
		feedAnimal(d);
		
		// 创建一只猫
		Cat c = new Cat();
		feedAnimal(c);
		
		Cat c1 =  (Cat)productAnimal(2);
		
	}
	
	/**
	 * 生成动物
	 * type = 1 表示生产狗
	 * type = 2 表示生产猫
	 */
	public static Animal productAnimal(int type) {
		if(type == 1)
			return new Dog();
		else if (type == 2) 
			return new Cat();
		return new Tiger();
	}
	
	/**
	 * 喂养动物
	 */
	public static void feedAnimal(Animal a) { //Animal a = new Dog();
		a.sleep();
		a.eat();
		
		// boolean b = a instanceof String;
		
		// boolean flag = 父类引用 instanceof 子类类名或接口名;
		// boolean flag = a instanceof Dog;
		// 判断a是否是狗
		if(a instanceof Dog) {
			// 将父类引用a转换为子类引用：Dog
			Dog d = (Dog)a;
			d.lookHome();
		} else if(a instanceof Cat) {
			// 将父类引用a转换为子类引用：Cat
			Cat c = (Cat)a;
			c.catchMouse();
		}
		
		// 调用狗表演的方法
		// 判断a是否实现了Perform
		if(a instanceof Perform) {
			Perform d = (Perform)a;
			d.show();
		}
	}
}
```


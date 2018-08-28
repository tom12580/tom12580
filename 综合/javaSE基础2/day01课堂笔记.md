# 学习目标

- 独立编码完成一个Person类的定义
- 能够动手操作Eclipse中的三个视图
- 能够阐述基本数据类型作为参数传递
- 能够阐述引用数据类型作为参数传递
- 能够解释this关键字的作用及用法
- 能够阐述匿名对象的使用格式及使用场景
- 独立编码定义商品项类
- 独立编码完成小票数据初始化功能
- 独立编码完成购物小票主干逻辑
- 独立编码完成为商品数量和金额赋值功能
- 独立编码完成购物小票打印功能

# 今天内容

## 1. 类定义的复习

```java
package com.itheima._01类定义;

// 成员变量 --> 构造方法 -->  普通方法 --> setter& getter
public class Person {
	// 成员变量
	private String name;
	private int age;
	
	// 构造方法
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
	public Person() {
	}
	// 成员方法：setter & getter
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
	// 普通方法
	public void eat() {
		System.out.println("年龄为"+this.age + "的"+this.name + "吃个蛋");
	}
}
```

## 2. 参数传递

- 基本数据类型
- 引用数据类型

```java

public class Demo01 {
	public static void main(String[] args) {
		// 定义一个整型变量
		int a = 100;
		print(a);
		System.out.println("a = " + a); // 100
		
		
		// 创建Person对象
		Person p = new Person("小泽", 23);
		// 调用print02方法
		print02(p);
		p.eat(); 
	}
	/**
	 * 引用数据类型作为参数传递：传递的是对象在内存中的地址值
	 */
	public static void print02(Person p) {
		p.eat();  // 
		p.setName("小苍");
		p.setAge(18);
	}
	
	/**
	 * 基本数据类型作为参数传递：传递的是变量的值
	 */
	public static void print(int a) {
		System.out.println("a = " + a); // 100
		// 对a加一
		a++;  
		System.out.println("a = " + a); // 101
	}
}

```

图解如下

![](image02.png)

## 3. this关键字

- this关键字的作用
  - 用来区分成员变量和局部变量同名的情况。
  - 代表`当前对象`的引用，当前对象指的是方法调用者。
    - 如果没有创建对象，则this关键字没有任何意义。
- this关键字的用法
  - 访问成员变量：this.成员变量名;
  - 调用成员方法：this.成员方法名(参数列表);
- 代码演示

```java
public class Demo01 {
	public static void main(String[] args) {
		// 创建两个Person对象
		Person p1 = new Person("林青霞",18);
		Person p2 = new Person("风清扬",23);
		
		// 调用eat方法
		p1.eat();
		p2.eat();
	}
}
```

![](image01.png)

## 4. 匿名对象

- 匿名对象概念
  - 没有名字的对象，只有创建对象的语法，没有将对象的地址值赋值给某一个引用变量。
  - 语法：new 类名();
- 匿名对象的特点
  - 使用一次之后就变成了垃圾对象，会在垃圾回收器(gc)空闲的时候回收该垃圾对象。
- 匿名对象的使用场景
  - 当对象只使用一次时，可以考虑使用匿名对象。
  - 作为实际参数传递

## 5. 超市购物小票案例

- 涉及到的知识点
  - 键盘录入对象：Scanner
  - 使用循环流程控制：for/while
  - 使用ArrayList集合存储所有的商品项
  - 字符串的切割方法：split方法
  - 判断语句：if/switch语句
  - 类定义和对象的创建。
  - IO流
  - 运算符的基本使用
- 实现步骤分析
  - 数据准备
    - 定义商品类和创建商品对象，存储到集合中。
  - 主界面的实现
    - 创建键盘录入对象
    - 接收用户输入的操作数据
    - 使用switch匹配用户输入的操作数。
  - 购买商品	 
    - 展示商品列表
    - 提示用户按照指定的格式输入购买信息
    - 接收用户输入的购买信息
      - 判断是否是end，如果是则结束购买。
    - 判断用户输入的购买信息是否符合格式
      - 如果不符合，则提示用户重新输入
    - 切割购买信息字符串，获得商品id
    - 查询商品是否存在
      - 如果不存在，则提示用户重新输入。
    - 判断购物车中是否已经存在该商品
      - 如果存在，则修改购买数量即可。
    - 创建一个新的商品对象，将新的商品添加到用户的购物车中(集合)
  - 结算并打印小票	
    - 判断购物车是否有商品
      - 如果没有，则提示用户先购买
    - 遍历购物车集合，输出商品的购买信息
    - 计算数量，价格等信息并输出。
  - 退出系统
    - System.exit(0)

![](image03.png)
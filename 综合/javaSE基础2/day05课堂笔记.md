# 学习目标
- 能够说出Object类中equals方法的作用
- 能够说出Object类中toString方法的作用
- 能够辨别程序中异常和错误的区别
- 说出异常的分类
- 说出虚拟机处理异常的方式
- 列举出常见的四个运行期异常
- 能够使用try...catch关键字处理异常
- 能够使用throws关键字处理异常				
- 能够自定义异常类

# 今天内容

- 异常概述和分类
- 异常的处理方式
- 异常注意事项与自定义异常

## 1. Object类中equals方法

```
 Object类中的equals方法
  	* boolean equals(Object obj) 
  		* 判断两个对象是否相同 	
  		* true：相同
  		* false：不同
  		
 Object类中equals方法的默认实现
 	* 通过==比较对象的地址值来判断是否相等。
 	
 重写equals方法的目的
 	* 因为比较两个对象通过地址值比较没有任何意义
 	* 一般比较两个对象是否相同是依据对象的成员变量值来比较。如果两个对象的成员变量值都相同，则认为是同一个对象。
```

测试代码

```java
/**
 * 学生类
 * @author pkxing
 *
 */
public class Student {
	private String name;
	private int age;
	
	public Student(String name, int age) {
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
	
	/*@Override
	public boolean equals(Object obj) { // obj = new Student();
		// this.name 和 obj.name
		// this.age 和obj.age
		
		// 判断obj是否为空
		if(obj == null) return false;
		// 判断obj是否是当前类的对象
		if(!(obj instanceof Student))  return false;
		
		if(obj == null || !(obj instanceof Student)) return false;
		if(this == obj) {
			return true;
		}
		// 向下转型
		Student s = (Student)obj;
		// 判断姓名是否相同
		if(!this.name.equals(s.name)) return false;
		// 判断年龄是否相同
		if(this.age != s.age) return false;
		// 姓名和年龄都相同
		return true;
	}*/
}

public class EqualsDemo01 {
	public static void main(String[] args) {
		
		// 创建两个字符串
		String str1 = new String("abc");
		String str2 = new String("abc");
		System.out.println(str1.equals(str2));
		
		
		// 创建学生对象：s1
		Student s1 = new Student("rose",18);
		// 创建学生对象：s2
		Student s2 = new Student("jack",18);
		s2 = null;
		// 判断对象s1和s2是否相同
		boolean b = s1.equals(s2); 
		/*
		  public boolean equals(Object obj) {
        			return this == obj;
    		   }
		 */
		System.out.println(b); // false
	}
}
```

## 2. Object类中toString()方法

```
Object类中的方法toString
		* String toString() 
    
Object类中的方法toString默认的方法值：	
    	* 类全名@对象在内存中地址值。 比如：com.itheima._02Object类方法.Student@33909752
    	重写toString方法的目的
    	* 在输出对象到控制台时，不希望看到对象在内存中的地址值，因为没有意义。
    	* 而实际开发中，一般希望在打印对象时查看该对象中所有成员变量的值是什么。
    		
 toString方法调用方式
    	* 直接调用：通过对象名调用
    	* 间接调用：打印对象到控制台时会自动调用。
```

测试代码

```java
/**
 * 学生类
 * @author pkxing
 *
 */
public class Student {
	private String name;
	private int age;
	
	public Student(String name, int age) {
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
		return "name=" + name + ", age=" + age;
	}
//	@Override
//	public String toString() {
//		return "我的姓名：" + this.name + ",我的年龄：" + this.age;
//	}
}
public class ToStringDemo {
	public static void main(String[] args) {
		String str = new String("abc");
		System.out.println(str); // abc
		
		// 创建学生对象
		Student s = new Student("rose",23);
		// System.out.println(s.getName()+"="+s.getAge());
		// 调用show方法
		// s.show();
		// 调用对象的toString
		System.out.println(s.toString()); 
		// 间接调用：直接输出对象：调用对象的toString方法
		System.out.println(s);
	}
}
```

## 3. 异常的概述
### 3.1. 什么是异常

- 在程序运行过程中出现的问题则称为异常。

### 3.2 异常的继承体系

- Throwable类：所有异常和错误的祖宗类。

  - Error：错误
  - Exception：异常

  ![](异常继承体系.png)

### 3.3 异常的分类

- 运行时异常
- 编译时异常

## 4. 异常与错误的区别

```java
错误
 * 在程序运行过程中出现的问题
 * 错误是操作系统产生的，反馈给JVM的，一般没有具体的处理方法，只能修改代码。否则程序直接奔溃。
 	
异常
 * 在程序运行过程或编译过程中出现的问题。
 * 异常是JVM产生的，反馈给程序猿的，可以针对异常进行处理，如果不处理，程序直接奔溃。 	
```

测试代码

```java
public class ExceptionDemo01 {
	public static void main(String[] args) {
		try {
			test01();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}
	public static void test01() {
		// 定义一个数组
		// 出现内存溢出错误，只能修改代码，否则程序崩溃
		// int[] arr = new int[1024 * 1024 * 1024];
		try {
			int result = 100/2;
			System.out.println(result);
		} catch(Exception e) {
			
			
		}
	}
}
```

## 5. Throwable常用方法

```java
void printStackTrace() 打印栈的异常信息
String getMessage() 获得异常的信息字符串
String toString() 获得异常的详细信息：类全名：异常信息字符串
```

示例代码

```java
public class ExceptionDemo02 {
	public static void main(String[] args) {
		test01();
	}
	
	public static void test01() {
		test02();
	}
	
	public static void test02() {
		// 创建Throwable对象：异常对象
		Throwable t = new Throwable("无病呻吟");
		// 调用方法
		t.printStackTrace();
		System.out.println(t.getMessage()); // 无病呻吟
		System.out.println(t.toString());  // java.lang.Throwable: 无病呻吟
		System.out.println("来了吗");
	}
}
```

## 6. 异常的处理方式

### 6.1 异常的处理方式

- JVM处理方式
  - 将异常的信息(异常的位置，异常类名，异常的信息)打印在控制台中。
  - 终止程序运行。
    - 一旦出现异常没有处理，出现异常代码后的代码都不会被执行了。


- 手动处理方式
  - 捕获处理
  - 抛出处理


   	单catch捕获处理方式格式

   		try {

   			

   		} catch(异常类名 变量名) {

   		

   		}

   	格式说明

   		* try代码块里编写可以出现异常的代码。

   		* catch代码块编写处理异常的代码。

   		* 如果try代码块中的代码没有出现异常，则catch代码块不会执行。


## 7. 异常处理之单catch捕获处理
### 7.1 单catch捕获处理格式

```java
try {

} catch(异常类名 变量名) {
   
}
格式说明
* try代码块里编写可以出现异常的代码。
* catch代码块编写处理异常的代码。
* 如果try代码块中的代码没有出现异常，则catch代码块不会执行。
```

### 7.2  单catch捕获示例代码

```java
public class ExceptionDemo04 {
	public static void main(String[] args) {
		test01(10);
	}
	
	public static void test01(int a) {
		try {
			// 可能会出现异常的代码
			/**
			 当a == 0 时
			 	1. 创建异常对象ArithmeticException
			 		ArithmeticException ee = new ArithmeticException("/ by zero");
			 	2. 抛出异常对象：ee
			 */
			int result = 100 / a; // a == 0 
			System.out.println(result);
		} catch(ArithmeticException  e) {  // e = ee
			System.out.println("进来了吗");
			e.printStackTrace();
		}
		System.out.println("来了吗");
	}
}
```

## 8. 异常处理之多catch捕获处理
### 8.1 多catch捕获处理格式

```java
try {
   			
} catch(异常类名 变量名) {
   		
} catch(异常类名 变量名) {
   		
} ....
 catch(异常类名 变量名) {
   		
}
格式说明
	* try代码块里编写可以出现异常的代码。
	* catch代码块编写处理异常的代码。
	* 如果try代码块中的代码没有出现异常，则catch代码块不会执行。
```

### 8.2  多catch捕获示例代码

```java
public class ExceptionDemo05 {
	public static void main(String[] args) {
		test01(2);
	}
    /*
    * 多catch异常处理
    */
	public static void test01(int a) {
		try {
			// 可能会出现异常的代码
			/**
			 当a == 0 时
			 	1. 创建异常对象ArithmeticException
			 		ArithmeticException ee = new ArithmeticException("/ by zero");
			 	2. 抛出异常对象：ee
			 */
			int result = 100 / a; 
			System.out.println(result);
			
			// 创建一个数组
			int[] arr = {1,2,35,56,4};
			/**
			 当索引越界时
			 	1. 创建异常对象ArrayIndexOutOfBoundsException
			 		  ee = new ArrayIndexOutOfBoundsException("索引值");
			 	2. 抛出异常对象：ee
			 */
			System.out.println(arr[4]);
			
			// 创建字符串
			String str = null;
			/**
			 当str=null时
			 	1. 创建异常对象NullPointerException
			 		  ee = new NullPointerException();
			 	2. 抛出异常对象：ee
			 */
			// System.out.println(str.length()); 
			
			str = "abc";
			/**
			 当字符串索引越界时
			 	1. 创建异常对象StringIndexOutOfBoundsException
			 		  ee = new StringIndexOutOfBoundsException(" String index out of range: 4");
			 	2. 抛出异常对象：ee
			 */
			System.out.println(str.charAt(4));
		} catch(ArithmeticException e) {  // e = new ArithmeticException("/by zero");
			System.out.println("进来了吗");
			e.printStackTrace();
		} catch(ArrayIndexOutOfBoundsException e) { // e =  new ArrayIndexOutOfBoundsException("5");
			System.out.println(e.toString());
			System.out.println("又进来了吗");
		} catch(NullPointerException e) { // e =  new NullPointerException();
			System.out.println("又又进来了吗");
		} catch(StringIndexOutOfBoundsException e) { // e =  new StringIndexOutOfBoundsException();
			System.out.println("又又又进来了吗");
		} 
		System.out.println("来了吗");
	}
}
```

## 9. 异常处理之多catch处理注意事项

- **平级关系**：异常类名之间没有先后顺序。


- **上下关系**：继承关系，父类类名要出现在子类类名的后面。

## 10. finallay代码块

- finally代码块的特点
  - 只要代码执行流程进入了try代码块，不管是否出现异常，都会执行finally代码块中的代码。


- finally代码块的作用	
  - 用来释放资源。比如关闭流，数据库资源等

示例代码

```java
public class ExceptionDemo06 {
	public static void main(String[] args) {
		// test01(0);
		System.out.println(test02());
	}
	
	public static int test02() { 
		try {
			System.out.println("test02");
			// System.exit(0);
			System.out.println(100/0);
			return 100;
		}finally {
			System.out.println("finallay");
			// return 200;
		}
	}

	public static void test01(int a) {
		if(a != 0) {
			try {
				// 可能会出现异常的代码
				int result = 100 / a; 
				System.out.println(result);
			
				// 创建流对象
				FileReader fr = new FileReader("a.txt");
				System.out.println(fr.read());
				
				fr.close();
			}  catch(Exception e) { 
				System.out.println("又又又进来了吗");
			} finally {
				// 关闭资源:io流，数据库资源
				System.out.println("执行了吗");
			} 
		}
		System.out.println("来了吗");
	}
}
```

## 11. 异常处理之抛出处理
### 11.1 throws和throw的作用

- **throw：**将异常对象抛出，抛给方法调用者，并结束当前方法运行。
- **throws：**用来声明方法可能会出现的异常，告诉方法调用者在调用方法时要对异常进行处理。

### 11.2 throws和throw使用格式

-  **throw：** throw 异常对象。


-  **throws：**修饰符 返回值类 方法名(参数列表) throws 异常类名1,异常类名2....异常类名n{}

### 11.3 throws和throw使用位置

-  **throws：**使用在方法声明上


- **throw：**使用方法体中使用

### 11.4 throws和throw注意事项

- throw后面的对象一定是异常类或子类对象。


- throws后面可以跟多个异常类名，多个异常类名之间使用逗号分隔。

### 11.5 示例代码

```java
public class ExceptionDemo07 {
	public static void main(String[] args) {
		try {
			test01(10);	
		} catch (Exception e) {
			System.out.println("执行了");
		}
		System.out.println("come here");
	}
	
	public static void test01(int a) throws ArithmeticException {
		if(a == 0) {
			// 抛一个数学运行异常
			throw new ArithmeticException("除数a不能为零");
		}
		int result = 100 / a; 
		System.out.println(result);
		System.out.println("来了吗");
	}
}
```

## 12. 方法重写时候异常的处理

```java
方法重写异常处理的注意事项(针对编译时异常而已的)
 	* 父类方法没有声明抛出异常，则子类重写方法是不能声明抛出异常。
 	* 父类方法声明一个异常时，子类重写方法时可以声明该异常的类型或子类类型。
 	* 父类方法声明多个异常时，子类重写方法时可以声明多个异常的子集。
小结:
	* 子类重写方法时不能声明比父类声明大的异常类型。
```

示例代码

```java
/**
* 父类
*/
public class Student {
	public void study()  throws IOException {
		System.out.println("父类学习方法");
	}
}
/**
* 子类
*/
public class BaseStudent extends Student {
	
	@Override
	public void study() throws FileNotFoundException{
		System.out.println("子类学习方法");
	}
}
```

## 13. 编译时异常和运行时异常

```java
运行时异常概念
  	* RuntimeException及其子类都属于运行时异常。
编译时异常概念
  	* 除了运行时异常以外的所有异常都属于编译时异常。
运行时异常的特点
  	* 方法内部抛出的异常是运行时异常时，则可以处理，也可以不处理。
  	* 方法声明上声明的异常是运行时异常时，则方法调用者可以处理，也可以不处理。
编译时异常的特点
  	* 方法内部抛出的异常是编译时异常，则要求必须要处理。
  	* 方法声明上声明的异常是编译时异常时，则要求方法调用者必要处理。
```

## 14. 自定义异常
### 14.1 自定义异常步骤

```java
异常类名：XxxException
继承异常类
	  * Exception：如果要求调用者一定要处理，则继承该异常。
	  * RuntimeException：如果不要求调用处理，则继承该异常。
提供构造方法
	  * 无参数构造
	  * 有参数构造 	
```

### 14.2 自定义异常格式

```java
public class XxxxException extends RuntimeException {
	public XxxxException() {
		
	}
	public XxxxException(String message) {
		super(message);
	}
}
```

### 14.3 自定义异常案例

* 在Person类的有参数构造方法中，进行年龄范围的判断，
* 若年龄为负数或大于200岁，则抛出NoAgeException异常，异常提示信息“年龄数值非法”。
* 要求：在测试类中，调用有参数构造方法，完成Person对象创建，并进行异常的处理。

```java
/**
 * 自定义异常类
 * @author pkxing
 *
 */
public class NoAgeException extends RuntimeException {
	public NoAgeException() {
		
	}
	public NoAgeException(String message) {
		super(message);
	}
}
/**
* 测试类
*/
public class ExceptionDemo09 {
	public static void main(String[] args) {
		// 创建Person对象
		try {
			Person p = new Person(200);
			p.setAge(300);
			System.out.println(p);
		} catch (NoAgeException e) {
			e.printStackTrace();
		}
	}
}
```

## 15. 常见问题
* 程序出现问题的时候如何判断是错误还是异常？
  * 通过类名判断，如果类名是Exception结尾，则是异常。如果是Error结尾，则是错误。
* 捕获异常时是不是捕获Exception即可？
  * 不是，因为在实际开发，一般都会针对不同的异常有不同的处理方式。
* 什么时候使用抛出处理？什么时候使用捕获处理?
  * 如果当前代码是直接跟用户交互的，则千万不能将异常对象抛出，应该捕获处理，然后在catch中输出友好信息给用户查看。
  * 如果当前代码不是直接跟用户交互，则可以捕获处理，但捕获处理之后一定要将异常抛给上一层。
* 父类方法没有声明抛出异常，子类覆盖方法时发生了异常，怎么办？
  * 子类自能自己捕获处理，如果子类处理完之后还想将异常告诉方法调用者，则只能使用throw关键字抛出运行时异常对象。
* 为什么java编译器对运行时异常管理的如此松散？
  -  因为运行时异常一般可以通过良好的编程习惯来避免。



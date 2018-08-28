

# 知识点回顾

```java
UDP协议相关的两个类
    DatagramPacket:用来封装要发送和接收的数据，类似集装箱
    	DatagramPacket(byte[] buf,int length,InetAddress address,int port); 创建发送端的数据包对象。
    	DatagramPacket(byte[] buf,int length); 创建接收端的数据包对象
    	
    	int getPort()；获得端口号
    	int getLength();获得实际接收的内容长度
    	InetAddress getAddress(); 获得IP地址对象。
    	
	DatagramSocket：用来发送和接收数据包对象，类似码头
		DatagramSocket(); 创建发送端的Socket
		DatagramSocket(int port); 创建接收端的Socket
		
        void send(DatagramPacket dp);发送数据包
        void recevie(DatagramPacket dp);接收数据包
	
TCP协议相关的两个类
	Socket：代表客户端
		Socket(String host,int port); 
			* 根据服务器地址和端口号创建客户端Socket对象
			* 一旦执行创建方法，就会立即连接服务器，如果连接失败，会抛出异常。
		OutputStream getOutputStream();获得字节输出流对象
		InputStream getInputStream();获得字节输入流对象
		void shutdownOutput(); 关闭输出流，告诉对方数据已经发送完毕。
		void close(); 关闭Socket对象释放资源。
		
	ServerSocket：代表服务器端
		ServerSocket(int port); 指定端口号开启服务器
		Socket accept(); 等待客户端连接并获得与客户端相关连的Socket
```



# 学习目标

- [ ] 能够使用Junit进行测试
- [ ] 能够使用类文件路径
- [ ] 能够使用Properties常用的方法
- [ ] 能够使用Properties生成配置文件与读取配置文件 
- [ ] 能够说出三种类加载器
- [ ] 能够说出每种类加载器的功能以及作用
- [ ] 能够说出动态代理模式的作用
- [ ] 能够使用Proxy的方法生成代理对象

# 今天知识点

- Junit
- Properties
- 类加载器
- 动态代理


# 第1章 Junit单元测试

```java
1.1 什么是Junit
	* Junit是第三方Java语言编写的单元测试框架

1.2 什么是单元测试
	* 在Java中，一个类就是一个单元。
	* 单元测试就是开发者编写的一小段代码，用来测试某个类中的某个方法的功能或业务逻辑是否正确的。 

1.3 Junit的作用
	* 帮助程序猿对类中的方法进行有目的的测试，保证方法的正确性和稳定性。
	* 可以让一个方法独立运行。

1.4 Junit的使用步骤
	* 编写业务类
	* 编写测试类 
		* 编写测试方法 	
	
	测试类的命名规则：
		* 以Test开头，以业务类名结尾。
		* 比如要测试的业务类：StudentDao，则测试类名：TestStudentDao

1.5 测试方法的特点
	* 声明要求
		* 要使用注解@Test修饰方法声明 
		* 必须使用public修饰，没有返回值，没有参数
	* 命名规则
		* 以test开头，以被测试的方法结尾，使用驼峰命名方法
		* 比如：要测试save，测试方法名：testSave();
	* 特点：可以独立运行。
		

1.6 如何运行测试方法
	* 选中方法：右键--> Run As --> 只执行选中的方法
	* 选中类名：右键--> Run As --> Junit Test 运行类中的所有测试方法
	* 选中项目名：右键--> Run As --> Junit Test 运行所有测试类中所有测试方法

1.7 查看测试结果
	* 绿色：测试通过
	* 红色：测试失败

1.8 常用注解
	@Before：该方法会在每一个测试方法执行之前执行一次
	@After：该方法会在每一个测试方法执行之后执行一次。
	@BeforeClass: 用来修饰静态方法，该方法会在所有的测试方法执行之前执行一次。
	@AfterClass: 用来修饰静态方法，该方法会在所有的测试方法执行之后执行一次。
```

- 示例代码

```java
public class TestStudentDao {
	// 创建业务类对象
	static StudentDao stuDao;
	
	@BeforeClass
	public static void init() {
		stuDao = new StudentDao();
		System.out.println("执行初始化操作");
	}
	
	@AfterClass
	public static void close() {
		stuDao = null;
		System.out.println("执行关闭资源操作");
	}
	
	/*@Before
	public void init() {
		stuDao = new StudentDao();
		System.out.println("执行初始化操作");
	}
	
	@After
	public void close() {
		stuDao = null;
		System.out.println("执行关闭资源操作");
	}*/
	
	/**
	 * 测试方法：可以独立运行。
	 */
	@Test public void testSave() {
		// 创建业务类对象
		// StudentDao stuDao = new StudentDao();
		boolean b = stuDao.save(new Student());
		// java.lang.AssertionError: 期望值和实际值不一致 expected:<false> but was:<true>
		/**
		 断言：预先判断某个条件一定成立，如果不成立则直接崩溃
		 expected:期望值
		 actual:实际值
		 message: 提示信息
		 */
		Assert.assertEquals("期望值和实际值不一致",true, b);
		System.out.println(b);
	}
	
	@Test public void testDelete() {
		// 创建业务类对象
		// StudentDao stuDao = new StudentDao();
		stuDao.delete(new Student());
		System.out.println("删除方法 ");
	}
}
```

# 第2章 Properties

```java
Properties集合的概述
	* 是一个双列集合，实现了Map接口，继承Hashtable。
	
Properties集合的特点
	* 不需要指定泛型
	* 键和值的类型都是字符串。
	* 可以和流技术相结合使用，可以直接将集合中的数据保存到文件中
	* 也可以从文件中读取数据到集中。

属性文件要求
	* 命名要求：xxx.properties
	* 文件中可以使用注释，注释是用 # 开头
	* 键和值的存储格式：键=值
	* 一个键值对占据一行
	
Properties集合常用方法
	* Object setProperty(String key,String value);存储键值对
	* String getProperty(String key);根据键获得值
	* Object remove(Object key); 根据键删除键值对
	* Set<String> stringPropertyNames(); 获得键的集合
	
	* store(OutputStream out, String comments) 
		* 将集合中的数据保存到流关联的文件中
		* comments：说明文字，一般给null即可 
	* void load(Reader reader) 从流关联的文件中读取数据到集合中。
```

- 示例代码

```java
public class PropertiesDemo {
	public static void main(String[] args) throws Exception{
		// 创建属性集合
		Properties pros = new Properties();
		pros.load(PropertiesDemo.class.getClassLoader().getResourceAsStream("jdbc.properties"));
		System.out.println(pros);
	}
	
	public static void test03() throws Exception{
		// 创建属性集合
		Properties pros = new Properties();
		System.out.println(pros);
		// 创建字节输出流
		// FileInputStream fis = new FileInputStream("aaa.properties");
		FileReader fis = new FileReader("bbb.properties");
		// 调用方法从文件中读取数据到集合中
		pros.load(fis);
		// 关闭流
		fis.close();
		System.out.println(pros);
	}
	
	
	public static void test02(String[] args) throws Exception{
		// 创建属性集合
		Properties pros = new Properties();
		pros.setProperty("name", "jack");
		pros.setProperty("gender", "男");
		pros.setProperty("age", "20");
		
		// 创建字节输出流
		// FileOutputStream fis = new FileOutputStream("aaa.properties");
		// 创建字符输出漏
		FileWriter fis = new FileWriter("bbb.properties");
		// 将集合中的数据保存到文件中
		pros.store(fis, null);
		fis.close();
	}
	
	public static void test01() {
		// 创建属性集合
		Properties pros = new Properties();
		// 添加键值对
		Object name =pros.setProperty("name", "jack");
		System.out.println(name);
		name =pros.setProperty("name", "rose");
		System.out.println(name);
		pros.setProperty("gender","男");
		
		// 获得值
		name = pros.getProperty("gender");
		System.out.println(name);
		System.out.println(pros);
		
		// 删除键值对
		pros.remove("gender");
		System.out.println(pros);
		
		System.out.println(pros.size());
		
		// 获得键集合
		Set<String> names = pros.stringPropertyNames();
		for (String n : names) {
			System.out.println(n+"=" + pros.getProperty(n));
		}
	}
}
```

# 第3章 类加载器

## 3.1 类加载器概述

### 3.1.1 什么是类加载器

- 一个负责加载类的对象。

### 3.1.2 类加载器的作用

- 将类的字节码文件从硬盘中加载到内存中，并在内存中创建了一个Class对象。
- Class对象就是根据字节码文件创建出来的，Class对象就包含了类的所有信息。

### 3.1.3 类加载器的分类

```java
* 引导类加载器：BootstrapClassLoader
    * 是由C和C++编写的，不是Java中的类 	
    * 负责加载Java中最基础最核心的类，比如：rt.jar
* 扩展类加载器：ExtClassLoader
    * 是Java语言编写的，是Java中的类，是ClassLoader的子类。 
    * 负责加载扩展包下的类
* 应用类加载器：AppClassLoader
    * 是Java语言编写的，是Java中的类，是ClassLoader的子类。 
    * 负责加载bin目录下的类和第三方jar中类。
```

## 3.2 类加载器的常用方法

```java
* ClassLoader getParent();
	* 获得父加载器对象(没有继承关系，只是上下级关系)
	* 应用类加载器的父加载器：ExtClassLoader
	* 扩展类加载器的父加载器：BootstrapClassLoader
	
* URL getResource(String name);
	* 根据资源文件名获得URL对象 
	* 默认从项目根目下的src文件夹下查找指定资源文件，(本质是bin目录下查找)
 	* URL ==> 统一资源定位符  http://www.baid.com/image/aaa.png
 
 	URL类的常用方法
 		* String getPath(); 获得资源的绝对路径
 	
* InputStream getResourceAsStream(String name);
	* 根据资源文件获得与该资源相关的字节输入流
	* 通过字节输入流对象就可以读取资源文件的内容
	* 默认从项目根目下的src文件夹下查找指定资源文件，(本质是bin目录下查找)
```

- 示例代码

```java
public class ClassLoaderDemo01 {
	public static void main(String[] args)  throws Exception{
		test01();
         test02()；
         test03()；
	}
	/**
	 * InputStream getResourceAsStream(String name);
	 */
	public static void test03()  throws Exception{
		// 获得URL对象
		ClassLoader cl = ClassLoaderDemo01.class.getClassLoader();
		InputStream in = cl.getResourceAsStream("a.txt");
		System.out.println(in.read());
	}
	/**
	 * URL getResource(String name);
	 */
	public static void test02()  throws Exception{
		// 获得URL对象
		ClassLoader cl = ClassLoaderDemo01.class.getClassLoader();
		// /Users/pkxing/Documents/Java%e5%ad%a6%e7%a7%91/mycodes/day17_%e8%af%be%e5%a0%82%e4%bb%a3%e7%a0%81/bin/a.txt
		URL url = cl.getResource("a.txt");
		System.out.println(url.getPath());
		// 创建字节输入流
		// FileInputStream fis = new FileInputStream("src/a.txt");
		FileInputStream fis = new FileInputStream(url.getPath());
		// 读取一个字节
		System.out.println(fis.read());
		// 关闭流
		fis.close();
	}
	/**
	 * ClassLoader getParent();
	 */
	public static void test01() {
		// 获得当前类的Class对象
		Class c = ClassLoaderDemo01.class;
		// 获得类加载器对象
		ClassLoader cl = c.getClassLoader();
		// 应用类加载器对象：sun.misc.Launcher$AppClassLoader@4e25154f
		System.out.println(cl);
		
		// 获得cl的父加载器对象
		ClassLoader clp = cl.getParent();
		// 扩展类加载器对象 ：sun.misc.Launcher$ExtClassLoader@33909752
		System.out.println(clp);
		
		// 获得clp的父加载器对象
		// null:表示是引导类加载器
		ClassLoader clpp = clp.getParent();
		System.out.println(clpp);
		
		// null：表示是引导类加载器
		System.out.println(String.class.getClassLoader()); 
	}
}
```

## 3.3 类加载器加载机制：双亲委托机制

### 3.3.1 双亲委派模型的工作流程

- 如果一个类加载器收到了类加载的请求，它首先不会自己去尝试加载这个类，而是把请求委托给父加载器去完成，依次向上，因此，所有的类加载请求最终都应该被传递到顶层的启动类加载器中，只有当父加载器在它的搜索范围中没有找到所需的类时，即无法完成该加载，子加载器才会尝试自己去加载该类。
  ![](类加载机制双亲委托机制.png)

# 第4章 动态代理

## 4.1 代理模式的作用

- 拦截对真实对象的直接访问，增强真实对象的功能。

## 4.2 代理模式的分类

- 静态代理(几乎不用了)
- 动态代理

## 4.3 动态代理案例演示

### 4.3.1 案例需求

- 现有一个数据访问层类(StudentDao)，类中提供了对学生进行增删改查的方法。
- 现有一个业务层类(StudentService)，该类需要调用StudentDao类中定义好的方法对学生进行增删改查操作。
- 要求：在调用StudentDao方法之前要先判断用户是否有权限
  - 如果有权限则允许调用StudentDao方法，并在调用完StudentDao方法之后要记录日志。
  - 如果没有权限则不允许调用StudentDao方法。
  - **不允许修改StudentDao类中代码实现以上需求。**

### 4.3.2 案例代码

- **定义Dao接口**

```java
// 用来描述增删改查的功能
public interface Dao<T>{
	// 增
	public void save(T t);
	// 删
	public void delete(T t);
	// 改
	public void update(T t);
	// 查
	public T find(int id);
}
```

- **数据访问层类StudentDao**

```java
/**
 * 对学生进行增删改查操作的类：数据访问层，直接跟数据库打交道
 * @author pkxing
 */
public class StudentDao implements Dao<Student>{

	@Override
	public void save(Student t) {
		System.out.println("增加学生信息:" + t);
	}

	@Override
	public void delete(Student t) {
		System.out.println("删除学生信息:" + t);
	}

	@Override
	public void update(Student t) {
		System.out.println("更新学生信息:" + t);
	}

	@Override
	public Student find(int id) {
		System.out.println("查询学生信息");
		Student s =  new Student();
		System.out.println(s);
		return s;
	}
}
```

- **业务层类StudentService**

```java

/**
	代码模式的作用
		* 拦截对真实对象的直接访问，增强真实对象的功能。
		
 	代理模式的分类
 		* 静态代理(几乎不用了)
 		* 动态代理
 	
	如何创建代理对象？
		* 通过Proxy类的静态方法创建代理对象
	
	Proxy类创建代理对象的方法
		* public static Object newProxyInstance(ClassLoader loader,
                             Class[] interfaces,
                             InvocationHandler h)
             * 创建代理对象
             * loader：类加载器，传递真实对象的类加载器即可
             * interfaces：接口类的Class对象数组，传递真实对象实现的接口的Class
             * h：回调处理对象，传递InvocationHandler接口实现类对象
            
            
    InvocationHandler接口的方法
    		* public Object invoke(Object proxy, Method method, Object[] args);
    			* 调用时机：每当通过代理对象调用方法时，都会被回调处理对象的该方法拦截。 
    			* proxy：代理对象 
    			* method：方法对象，代理对象调用的方法，被封装成了Method对象。
    			* args：参数，代理对象调用方法时传递的参数。
    			
    动态代理的使用步骤
    		* 先明确要代理的功能(方法)有哪些？
    		* 把要代理的功能(方法)定义在接口中
    		* 让被代理对象(真实对象)实现接口，重写接口中的方法。
    		* 创建被代理对象(不要直接通过被代理对象调用方法了)
    		* 通过Proxy类提供的静态方法创建代理对象
    			* 传递真实对象的类加载器对象
    			* 传递真实对象实现的接口对应的Class对象数组。
    			* 传递一个回调处理对象
    		* 通过代理对象调用接口中方法，在回调处理对象的invoke方法来根据拦截到的方法
    			判断是否需要执行真实对象的方法。
    	
    	注意事项
    		* 不要在invoke方法中通过代理调用方法，很容易出现递归死循环，最终导致程序出现内存溢出错误。
 */
public class StudentService {
	public static void main(String[] args) {
		// 创建学生对象
		Student stu = new Student("jack",23);
		// 创建数据访问层对象
		// 真实对象(被代理的对象)
		final StudentDao stuDao = new StudentDao();
		
		///Class[] interfaces = new Class[] {Dao.class,List.class};
		// String[] strs = new String[] {"abc"};
		
		/**
		 class ProxyImp implements Dao{
		 		// 增
				public void save(T t) {
				
				}
				// 删
				public void delete(T t){
				
				}
				// 改
				public void update(T t){
				
				}
				// 查
				public T find(int id) {
				
				}
		 }
		 
		 return new ProxyImp();
		 */
		// 代理对象
		/*Dao proxy = (Dao)Proxy.newProxyInstance(
				StudentDao.class.getClassLoader(),
				StudentDao.class.getInterfaces(), 
				new MyInvocationHandler(stuDao));*/
		Dao proxy = (Dao)Proxy.newProxyInstance(
				StudentDao.class.getClassLoader(),
				StudentDao.class.getInterfaces(), 
				new InvocationHandler() {
					/**
					 * 记录日志
					 */
					public void log() {
						System.out.println("记录日志");
					}
					
					/**
					 * 检查权限
					 */
					public boolean check() {
						System.out.println("检查权限");
						return true;
					}
					
					/**
					 该方法的调用时机：每当通过代理对象调用方法时，都会被回调处理对象的该方法拦截。 
					 
					  public Object invoke(Object proxy, Method method, Object[] args);
				    			* proxy：代理对象 
				    			* method：方法对象，代理对象调用的方法，被封装成了Method对象。
				    			* args：参数，代理对象调用方法时传递的参数。
				    			* 返回值：调用真实对象后返回值
					 */
					@Override
					public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
						// System.out.println("来了：" + method.getName() + ",args = " + Arrays.toString(args));
						// 判断是否有权限
						if(check()) {
							// 有权限，则调用真实对象(被代理对象)的方法
							Object result = method.invoke(stuDao, args);
							// 记录日志
							log();
							return result;
						} else {
							// 如果没有权限，不需要调用真实对象方法
						}
						return null;
					}
				});
		
		// 对学生进行增删改查
		proxy.save(stu);
		proxy.delete(stu);
		proxy.update(stu);
		Student s = (Student) proxy.find(1);
		
		FileFilter proxy2 = (FileFilter) proxy;
		// proxy2.accept(pathname)
		
		System.out.println(s);
	}
}

/**
 * 自定义回调处理对象
 * @author pkxing
 *
 */
class MyInvocationHandler implements InvocationHandler {
	
	// 成员变量：用来接收真实对象
	private Dao stuDao;
	
	public MyInvocationHandler(Dao stuDao) {
		this.stuDao = stuDao;
	}

	/**
	 * 记录日志
	 */
	public void log() {
		System.out.println("记录日志");
	}
	
	/**
	 * 检查权限
	 */
	public boolean check() {
		System.out.println("检查权限");
		return true;
	}
	
	/**
	 该方法的调用时机：每当通过代理对象调用方法时，都会被回调处理对象的该方法拦截。 
	 
	  public Object invoke(Object proxy, Method method, Object[] args);
    			* proxy：代理对象 
    			* method：方法对象，代理对象调用的方法，被封装成了Method对象。
    			* args：参数，代理对象调用方法时传递的参数。
    			* 返回值：调用真实对象后返回值
	 */
	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		// System.out.println("来了：" + method.getName() + ",args = " + Arrays.toString(args));
		// 判断是否有权限
		if(check()) {
			// 有权限，则调用真实对象(被代理对象)的方法
			Object result = method.invoke(stuDao, args);
			// 记录日志
			log();
			return result;
		} else {
			// 如果没有权限，不需要调用真实对象方法
		}
		return null;
	}
}
```

## 4.4 动态代理开发步骤

```java
* 先明确要代理的功能(方法)有哪些？
* 把要代理的功能(方法)定义在接口中
* 让被代理对象(真实对象)实现接口，重写接口中的方法。
* 创建被代理对象(不要直接通过被代理对象调用方法了)
* 通过Proxy类提供的静态方法创建代理对象
    * 传递真实对象的类加载器对象
    * 传递真实对象实现的接口对应的Class对象数组。
    * 传递一个回调处理对象
* 通过代理对象调用接口中方法，在回调处理对象的invoke方法来根据拦截到的方法
    判断是否需要执行真实对象的方法。
```


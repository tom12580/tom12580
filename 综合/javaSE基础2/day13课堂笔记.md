# 学习目标

- 能够独立完成统计每个班级的平均分和总分的案例


- 能够独立写出文件夹递归拷贝案例


- 能够说出选择排序和冒泡排序的原理


- 能够写出选择排序和冒泡排序的代码


- 能够说出枚举的作用


- 能够定义枚举类


- 能够应用枚举类

# 今天知识

* 集合和IO练习
* 排序算法
* 枚举类
* 自定义对象输出流

## 1. 集合练习
### 1.1 需求说明

```java
(1)定义一个学生类Student，属性：姓名(String name)、班级班号(String class_number)、分数(double score)
(2)初始化数据将若干Student对象存入List集合
(3)以班级为单位,使用Map存储所有该班学生
(4)统计每个班级的总分和平均分
```

### 1.2 代码实现

```java
/**
 定义一个学生类Student，属性：姓名(String name)、班级班号(String class_number)、
分数(double score)
 */
public class Student {
	private String name;
	private String className;
	private double score;
	
	public Student() {
		super();
	}
	public Student(String name, double score,String className) {
		super();
		this.name = name;
		this.className = className;
		this.score = score;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getClassName() {
		return className;
	}
	public void setClassName(String className) {
		this.className = className;
	}
	public double getScore() {
		return score;
	}
	public void setScore(double score) {
		this.score = score;
	}
	@Override
	public String toString() {
		return "Student [name=" + name + ", className=" + className + ", score=" + score + "]";
	}
}
// 方式1
public class Demo01 {
	public static void main(String[] args) {
		// 定义班级名称字符串
		String className82 = "JavaEE82期就业班";
		String className83 = "JavaEE83期就业班";
		// 创建班级集合
		List<Student> class82 = new ArrayList<>();
		List<Student> class83 = new ArrayList<>();
		
		// 创建82期学生对象
		Student s1 = new Student("鲁迅",98,className82);
		Student s2 = new Student("李大钊",88,className82);
		
		// 创建83期学生对象
		Student s3 = new Student("陈独秀",100,className83);
		Student s4 = new Student("杨秀琴",78,className83);
		
		// 将学生添加对应的班级集合中
		Collections.addAll(class82, s1,s2);
		Collections.addAll(class83, s3,s4);
		
		// 创建年纪集合Map，键：班级名称  值：班级学生集合
		Map<String,List<Student>> classes = new HashMap<String,List<Student>>();
		classes.put(className82, class82);
		classes.put(className83, class83);
		
		// 统计每个班级的总分和平均分
		Set<Entry<String,List<Student>>> entrySet = classes.entrySet();
		// 获得迭代器对象
		Iterator<Entry<String,List<Student>>> it = entrySet.iterator();
		while(it.hasNext()) {
			// 获得Entry对象
			Entry<String,List<Student>> entry = it.next();
			// 获得班级名
			String className = entry.getKey();
			// 获得班级集合
			List<Student> stus = entry.getValue();
			// 定义一个变量用来统计该班级的总分
			double totalScore = 0;
			// 遍历班级集合
			for (Student stu : stus) {
				// 累加总分
				totalScore += stu.getScore();
			}
			// 求该班级平均分
			double avg = totalScore / stus.size();
			System.out.println(className + " 班级的总分：" + totalScore+",平均分：" + avg);
		}
	}
}

// 方式2

/**
 * 班级类，一个该类的对象就代表一个班级
 * @author pkxing
 *
 */
public class ClassRoom {
	private String name;
	private String address;
	private List<Student> stus;
	
	public ClassRoom(String name, String address, List<Student> stus) {
		super();
		this.name = name;
		this.address = address;
		this.stus = stus;
	}
	public ClassRoom() {
		super();
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getAddress() {
		return address;
	}
	public void setAddress(String address) {
		this.address = address;
	}
	public List<Student> getStus() {
		return stus;
	}
	public void setStus(List<Student> stus) {
		this.stus = stus;
	}
	@Override
	public String toString() {
		return "ClassRoom [name=" + name + ", address=" + address + ", stus=" + stus + "]";
	}	
}

public class Demo02 {
	public static void main(String[] args) {
		// 定义班级名称字符串
		String className82 = "JavaEE82期就业班";
		String className83 = "JavaEE83期就业班";
		// 创建班级集合
		List<Student> class82 = new ArrayList<>();
		List<Student> class83 = new ArrayList<>();
		
		// 创建82期学生对象
		Student s1 = new Student("鲁迅",98,className82);
		Student s2 = new Student("李大钊",88,className82);
		
		// 创建83期学生对象
		Student s3 = new Student("陈独秀",100,className83);
		Student s4 = new Student("杨秀琴",78,className83);
		
		// 将学生添加对应的班级集合中
		Collections.addAll(class82, s1,s2);
		Collections.addAll(class83, s3,s4);
		
		// 创建班级对象
		ClassRoom cr82 = new ClassRoom(className82, "308", class82);
		ClassRoom cr83 = new ClassRoom(className83, "207", class83);
		
		// 创建年纪集合Map，键：班级名称  值：班级对象
		Map<String,ClassRoom> classes = new HashMap<String,ClassRoom>();
		classes.put(className82, cr82);
		classes.put(className83, cr83);
		
		// 使用keySet方法遍历
		Set<String> keySet = classes.keySet();
		for (String className : keySet) {
			// 根据班级名称获得班级对象
			ClassRoom cr = classes.get(className);
			// 获得班级集合
			List<Student> stus = cr.getStus();
			// 定义一个变量用来统计该班级的总分
			double totalScore = 0;
			// 遍历班级集合
			for (Student stu : stus) {
				// 累加总分
				totalScore += stu.getScore();
			}
			// 求该班级平均分
			double avg = totalScore / stus.size();
			System.out.println(className + " 班级的总分：" + totalScore+",平均分：" + avg);
		}
	}
}
```

## 2. IO练习
### 2.1 需求说明

```java
将一个文件夹中的内容(包含子文件夹中的所有内容)复制到指定目的地
- 要求不允许使用CommonsIO
```

### 2.2 代码实现

```java
public class IODemo01 {
	public static void main(String[] args) {
		// 创建源文件夹
		File srcDir = new File("/Users/pkxing/documents/83期就业班");
		// 创建目标文件夹
		File destDir = new File("/Users/pkxing/documents/aaa");
		long start = System.currentTimeMillis();
		// 文件夹复制
		copyDir(srcDir,destDir);
		System.out.println(System.currentTimeMillis() - start);
	}
	
	/**
	 * 文件夹复制
	 * @param srcDir  源文件夹
	 * @param destDir 目标文件夹
	 */
	public static void copyDir(File srcDir,File destDir) {
		// 判断源文件夹是否存在
		if(!srcDir.exists()) return;
		// 判断是否是文件夹
		if(!srcDir.isDirectory()) return;
		// 创建目标文件夹
		destDir.mkdirs();

		// 获得源文件夹下的所有文件
		File[] files = srcDir.listFiles();
		// 遍历文件数组
		for (File file : files) {
			File destFile = new File(destDir,file.getName());
			// 判断是否是文件夹
			if(file.isDirectory()) {
				// destFile = /Users/pkxing/documents/aaa/day01
				// 递归调用当前方法
				copyDir(file,destFile);
			} else {
				// 创建目标文件
				// /Users/pkxing/documents/aaa/广州黑马JavaEE就业83期（20180403面授）学员点招考试成绩.xls
				// File destFile = new File(destDir,file.getName());
				// 文件复制
				copyFile(file, destFile);
			}
		}
	}
	
	
	/**
	 * 文件复制
	 * @param srcFile 要复制的文件
	 * @param destFile 目标文件
	 */
	public static void copyFile(File srcFile,File destFile) {
		try(
			// 创建字节缓冲输入流	
			BufferedInputStream bis = new BufferedInputStream(new FileInputStream(srcFile));
			// 创建字节缓冲输出流
			PrintStream ps =	new PrintStream(new BufferedOutputStream(new FileOutputStream(destFile)));
			){
			// 定义字节数组用来存储读取到的字节
			byte[] b = new byte[8192];
			// 定义一个变量用来接收读取到的字节数
			int len = -1;
			// 循环读写文件
			while((len = bis.read(b)) != -1) {
				// 输出目标文件
				ps.write(b, 0, len);
			}
		}catch(Exception e) {
			throw new RuntimeException(e);
		}
	}
	
	/**
	 * 文件复制
	 * @param srcFile 要复制的文件
	 * @param destFile 目标文件
	 */
	public static void copyFile01(File srcFile,File destFile) {
		try(
			// 创建字节缓冲输入流	
			BufferedInputStream bis = new BufferedInputStream(new FileInputStream(srcFile));
			// 创建字节缓冲输出流
			BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(destFile))
			){
			// 定义字节数组用来存储读取到的字节
			byte[] b = new byte[8192];
			// 定义一个变量用来接收读取到的字节数
			int len = -1;
			// 循环读写文件
			while((len = bis.read(b)) != -1) {
				// 输出目标文件
				bos.write(b, 0, len);
			}
		}catch(Exception e) {
			throw new RuntimeException(e);
		}
	}
	
}
```

## 3. 冒泡排序

### 3.1 原理

- 比较两个相邻的元素，将值大的元素交换至右端。

### 3.2 思路

```java
依次比较相邻的两个数，将小数放在前面，大数放在后面。
即在第一趟：首先比较第1个和第2个数，将小数放前，大数放后。
然后比较第2个数和第3个数，将小数放前，大数放后，如此继续，
直至比较最后两个数，将小数放前，大数放后。重复第一趟步骤，
直至全部排序完成。
```

### 3.3 需求

```java
1. 现有数组和集合如下
  int[] arr={6,3,8,2,9,1}
  List<Integer> list = new ArrayList<Integer>();
  Collections.addAll(list,6,3,8,2,9,1);
2. 使用冒泡排序对数组和集合元素进行排序。
```

### 3.4 代码实现

```java
public class BubbleSortDemo01 {
	public static void main(String[] args) {
		test01();
		test02();
	}
	// 冒泡排序：对集合元素排序
	public static void test02() {
		// 数组
		List<Integer> list = new ArrayList<Integer>();
		Collections.addAll(list,6,3,8,2,9,1);
		System.out.println(list);
		for(int i = 0; i < list.size() - 1; i++ )  {  // 外循环：控制趟数
			for(int j = 0; j < list.size() - 1 - i; j++) {  // 内循环：控制次数
				if(list.get(j) > list.get(j+1)) {
					// 定义临时变量temp
					int temp = list.get(j);
					list.set(j, list.get(j+1));
					list.set(j+1, temp);
				}
			}
		}
		System.out.println(list);
	}
	
	// 冒泡排序：对数组进行排序
	public static void test01() {
		// 数组
		int[] arr={6,3,8,2,9,1};
		// System.out.println(Arrays.sort(a)); // 快速排序算法
		System.out.println(Arrays.toString(arr));
		for(int i = 0; i < arr.length - 1; i++ )  {  // 外循环：控制趟数
			for(int j = 0; j < arr.length - 1 - i; j++) {  // 内循环：控制次数
				if(arr[j] > arr[j+1]) {
					// 定义临时变量temp
					int temp = arr[j];
					arr[j] = arr[j+1];
					arr[j+1] = temp;
				}
			}
		}
		System.out.println(Arrays.toString(arr));
	}
}
```

## 4. 选择排序

### 4.1 原理

```java
首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。以此类推，直到所有元素均排序完毕。
```

### 4.2 思路

```java
给定数组：int[] arr={里面n个数据}；
第1趟，在待排序数据arr[1]~arr[n]中选出最小的数据，将它与arrr[0]交换；
第2趟，在待排序数据arr[2]~arr[n]中选出最小的数据，将它与arr[1]交换；
以此类推，第i趟在待排序数据arr[i]~arr[n]中选出最小的数据，将它与arr[i-1]交换，直到全部排序完成。
```

### 4.3 需求

```java
1. 现有数组和集合如下
  int[] arr={6,3,8,2,9,1}
  List<Integer> list = new ArrayList<Integer>();
  Collections.addAll(list,6,3,8,2,9,1);
2. 使用冒泡排序对数组和集合元素进行排序。
```

### 4.4 代码实现

```java
/**
给定数组：int[] arr={里面n个数据}；
第1趟，在待排序数据arr[1]~arr[n]中选出最小的数据，将它与arrr[0]交换；
第2趟，在待排序数据arr[2]~arr[n]中选出最小的数据，将它与arr[1]交换；
以此类推，第i趟在待排序数据arr[i]~arr[n]中选出最小的数据，将它与arr[i-1]交换，直到全部排序完成。
	int[] arr={6,3,8,2,9,1};
	for(int i = 0; i < arr.length - 1;i ++){ // 外循环：控制趟数
		for(int j = i+1 ; j <= arr.length - 1 ; j++){  // 内循环：控制比较次数
			
		}
	}
	第1趟
		第1次 3,6,8,2,9,1
		第2次 3,6,8,2,9,1
		第3次 2,6,8,3,9,1
		第4次 2,6,8,3,9,1
		第5次 1,6,8,3,9,2
		
	第2趟
		第1次 1,6,8,3,9,2
		第2次 1,3,8,6,9,2
		第3次 1,3,8,6,9,2
		第4次 1,2,8,6,9,3
		
	第3趟
		第1次 1,2,6,8,9,3
		第2次 1,2,6,8,9,3
		第3次 1,2,3,8,9,6
		
	第4趟
		第1次 1,2,3,8,9,6
		第2次 1,2,3,6,9,8
	
	第5趟
		第1次 1,2,3,6,8,9

 */
public class SelectionSortDemo {
	public static void main(String[] args) {
		test01();
		test02();
	}
	// 选择排序：对数组元素排序
	public static void test01() {
		int[] arr={6,3,8,2,9,1};
		System.out.println(Arrays.toString(arr));
		for(int i = 0; i < arr.length - 1;i ++){ // 外循环：控制趟数
			for(int j = i+1 ; j < arr.length; j++){  // 内循环：控制比较次数
				if(arr[i]  > arr[j]) {
					int temp = arr[i];
					arr[i] = arr[j];
					arr[j] = temp;
				}
			}
		}
		System.out.println(Arrays.toString(arr));
	}
	// 选择排序：对集合元素排序
	public static void test02() {
		// 数组
		List<Integer> list = new ArrayList<Integer>();
		Collections.addAll(list,6,3,8,2,9,1);
		System.out.println(list);
		for(int i = 0; i < list.size() - 1; i++ )  {  // 外循环：控制趟数
			for(int j = i+1; j < list.size(); j++) {  // 内循环：控制次数
				if(list.get(i) > list.get(j)) {
					// 定义临时变量temp
					int temp = list.get(i);
					list.set(i, list.get(j));
					list.set(j, temp);
				}
			}
		}
		System.out.println(list);
	}
}
```

## 5. 枚举类概述和定义格式

```java
枚举类概述
	* JDK1.5新特性
	* 枚举类本质就是一个类。
	* 所有自定义的枚举类都是Enum类的子类。

枚举类的定义格式
	* enum 类名 {
		// 定义枚举值
	 }
	
枚举类的注意事项
	* 构造方法必须使用private修饰。
	* 枚举值必须是枚举类的第一行有效语句。
	* 枚举值之间必须使用逗号分隔，最后一个枚举值以分号结束。
	
枚举类的作用
	* 提高了代码的可读性。
	* 可以限制一种数据类型的取值在给定的范围内选择，避免产生垃圾值。
	
枚举类的使用场景
	* 当某种数据类型的值只能在给定的范围内进行选择时，就应该使用枚举类进行限制
		* 比如：季节，月分，星期,方向,性别... 	
	
枚举类的成员方法
	* String name(); 获得枚举值的名字
	* static 枚举类型 valueOf(String name);  将字符串转换为枚举类对象
```

- 示例代码

```java
public class EnumDemo01 {
	public static void main(String[] args) {
		// 创建学生对象
		Student s = new Student("rose",Gender.valueOf("MAN"));
		
		// 输出枚举值名字
		System.out.println(Gender.MAN.name()); // MAN
		
		// SUNDAY
		Gender gender = Gender.valueOf("MAN");
		System.out.println(gender);
		System.out.println(s);
	}
}

// 定义一个枚举类：性别类
enum Gender{
	MAN("男"), // public static final Gender MAN = new Gender("男");
	WOMAN("女");  // public static final Gender WOMAN = new Gender("女");
	// MAN("男"),WOMAN("女");
	
	// 定义成员变量
	private String info;
	private Gender() {
		
	}
	private Gender(String info) { 
		System.out.println(info);
		this.info = info;
	}
	
	@Override
	public String toString() {
		return this.info;
	}
}

// JDK1.5之前的实现方案： 定义一个类：性别类
/*class Gender {
	private String info;
	// 男
	public static final Gender MAN = new Gender("男");
	// 女
	public static final Gender WOMAN = new Gender("女");
	
	private Gender(String info) {
		this.info = info;
	}
	private Gender() {
	}

	public String getInfo() {
		return info;
	}

	public void setInfo(String info) {
		this.info = info;
	}

	@Override
	public String toString() {
		return info;
	}
}*/

// 学生类
class Student{
	private String name;
	private Gender gender;
	public Student(String name, Gender gender) {
		this.name = name;
		setGender(gender);
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
	public Gender getGender() {
		return gender;
	}
	public void setGender(Gender gender) {
		this.gender = gender;	
	}
	@Override
	public String toString() {
		return "Student [name=" + name + ", gender=" + gender + "]";
	}	
}
```

## 6. 自定义对象输出流

```java
/**
 * 自定义对象输出流
 * @author pkxing
 *
 */
public class MyObjectOutputStream  extends ObjectOutputStream{
	// 定义成员变量接收目标文件对象
	private static File file;
	
	protected MyObjectOutputStream(OutputStream out) throws IOException {
		super(out);
	}
	
	public static MyObjectOutputStream getInstance(File file,boolean append) throws IOException {
		// 记录目标文件
		MyObjectOutputStream.file = file;
		return new MyObjectOutputStream(new FileOutputStream(file,append));
	}
	
	/**
	 * 重写该方法判断是否有输出头部信息
	 */
	@Override
	protected void writeStreamHeader() throws IOException {
		// 判断是否是第一次，如果是，调用父类的该方法，如果不是，则什么都不做
		if(!file.exists() || file.length() == 0) {
			System.out.println("是第一次吗");
			super.writeStreamHeader();
		} 
	}
}


/**
 	要实现对象输出流追加输出对象的数据，则要保证第一次写出对象时要写出一个头部信息，之后再次写出对象时
 	不能再写出头部信息
 	
 	如何实现只在第一次写出头部信息？ 
 		* 自定义对象输出流的子类，重写writeStreamHeader()方法
 			* 判断是否是第一次，如果是则写出头部信息，调用父类的writeStreamHeader()。
 			* 如果不是第一次，则不该方法中不执行任何代码
 */
public class ObjectOutputStreamDemo {
	public static void main(String[] args) throws Exception{
		// 创建自定义对象输出流
		MyObjectOutputStream oos =  MyObjectOutputStream.getInstance(new File("stu.txt"),false);
		// 创建学生对象
		Student s = new Student("jack",23);
		oos.writeObject(s);
		// 关闭流
		oos.close();
	}
	
	public static void test01() throws Exception{
		// 创建文件对象
		File file = new File("stu.txt");
		// 创建对象输出流
		ObjectOutputStream oos = null;
		// 判断文件是否存在或文件长度是否为0
		if(!file.exists() || file.length() == 0) {
			// 第一次：要输出头部信息
			// 创建对象输出流
			oos = new ObjectOutputStream(new FileOutputStream(file,true));
		} else {
			// 不能输出头部信息
			oos = new MyObjectOutputStream(new FileOutputStream(file,true));
		}
		// 创建学生对象
		Student s = new Student("jack",23);
		oos.writeObject(s);
		// 关闭流
		oos.close();
	}
}

```


# 学习目标
* 能够说出Map集合特点
* 使用Map集合添加方法保存数据
* 使用”键找值”的方式遍历Map集合
* 使用”键值对”的方式遍历Map集合
* 能够使用HashMap存储自定义键值对的数据
* 能够说出可变参数的作用
* 能够使用集合工具类
* 能够使用HashMap编写斗地主洗牌发牌案例	

# 今日内容

- Map集合
- 可变参数
- Arrays和Collections工具类
- 斗地主案例完善

## 1. Map集合概述

```java
Map接口概述
	* 是一个双列集合，存储元素时一次存储两个元素。
	* 一个元素称为键：key
	* 一个元素称为值：value
	* 键和值是成对出现的，统称键值对。
	
Map集合的特点
	* 键必须唯一
	* 值可以重复

Map接口常用实现类
	* Hashtable(了解)
	* HashMap
	* LinkedHashMap
```

## 2. Map接口中的常用方法

```java
* V put(K key, V value) 
     * 存储键值对
     * 如果键已经存在，则使用新值替换旧值，返回旧值。
     * 如果键不存在，则直接存储键值对，返回null

* V remove(Object key); 
     * 根据键删除键值对，返回键对应的值
     * 如果键不存在，则返回null

* V get(Object key); 
    * 根据键获得对应的值 
    * 如果键不存在，则返回null

* int size();
     * 获得键值对个数 
* Set<K> keySet(); 获得键集合
```

- 示例代码

```java
public class MapDemo01 {
	public static void main(String[] args) {
		// 创建Map集合
		Map<String,String> map = new HashMap<String,String>();
		// 存储元素
		String name = map.put("name", "张三");
		System.out.println(name); // null
		map.put("gender", "男");
		map.put("hobby", "撸代码");
		
		name = map.put("name", "李四");
		System.out.println(name); // 张三
		
		// 删除元素
		String value = map.remove("name");
		System.out.println("value = " + value);
		
		// 根据键获得值
		String gender = map.get("gender");
		System.out.println(gender);
		
		// 获得键值对个数
		System.out.println(map.size()); // 2
		
		System.out.println(map); 
	}
}
```

## 3. Map集合遍历方式keySet方法

  * 通过调用map集合的keySet方法获得键的集合
  * 通过增强for或迭代器遍历键集合
  * 通过键获得对应的值

```java
/**
 	Map集合不能直接使用迭代器或增强for遍历。
 	
	Map集合的遍历方式1：通过keySet方式遍历
		* 通过调用map集合的keySet方法获得键的集合
		* 通过增强for或迭代器遍历键集合
		* 通过键获得对应的值
 */
public class MapDemo02 {
	public static void main(String[] args) {
		// 创建Map集合
		Map<String,String> map = new HashMap<String,String>();
		// 存储元素
		map.put("name", "张三");
		map.put("gender", "男");
		map.put("hobby", "撸代码");
		
		// 获得所有的键
		Set<String> keySet = map.keySet();
		// 使用增强for遍历键集合
		for (String key : keySet) {
			System.out.println(key+"="+map.get(key));
		}
		// 使用迭代器遍历键集合
		Iterator<String> it =  keySet.iterator();
		while(it.hasNext()) {
			// 获得键
			String key = it.next();
			// 根据键获取值
			System.out.println(key+"=" + map.get(key));
		}
	}
}
```

## 4. Map集合Entry对象

```java
Entry对象的概述
    * 每一个键值对都会被封装成一个Entry对象

Entry对象常用方法
    * K getKey(); 获得键
    * V getValue(); 获得值
```

![](Entry对象.png)

## 5. Map集合遍历方式entrySet方法

 * 通过调用map集合的entrySet方法获得Entry对象集合。
 * 通过增强for或迭代器遍历Entry对象集合。
 * 通过调用Entry对象的getKey方法获得键，调用getValue方法获得值。

```java
public class MapDemo03 {
	public static void main(String[] args) {
		// 创建Map集合
		Map<String,String> map = new HashMap<String,String>();
		// 存储元素
		map.put("name", "张三"); // new Entry("name","张三");
		map.put("gender", "男");
		map.put("hobby", "撸代码");
		
		// 获得entry对象的集合
		Set<Entry<String,String>> entrySet =  map.entrySet();
		// 增强for遍历entrySet集合
		for (Entry<String, String> entry : entrySet) {
			// 获得键
			String key = entry.getKey();
			// 获得值
			String value  = entry.getValue();
			System.out.println(key +"=" + value);
		}
		System.out.println("----------");
		// 使用迭代器遍历entrySet集合
		Iterator<Entry<String,String>> it = entrySet.iterator();
		while(it.hasNext()) {
			// 获得entry对象
			Entry<String,String> entry = it.next();
			System.out.println(entry.getKey() + "=" + entry.getValue());
		}
	}
}
```

## 6. HashMap集合存储和遍历自定义对象

```java
/**
  使用Map存储自定义对象
 */
public class MapDemo05 {
	public static void main(String[] args) {
		// test01();
		test02();
	}
	/**
	 * 存储自定义对象：键是自定义对象，值是字符串
	 */
	public static void test02() {
		// 创建集合对象
		Map<Student,String> map = new HashMap<>();
		map.put(new Student("华安",23),"东莞");
		map.put(new Student("小泽",24),"东京");
		map.put(new Student("小苍",30),"日本");
		map.put(new Student("小苍",30),"深圳");
		map.put(new Student("小野",23),"广州");
		System.out.println(map);
	}
	
	/**
	 * 存储自定义对象：键是字符串，值是自定义对象
	 */
	public static void test01() {
		// 创建集合对象
		Map<String,Student> map = new HashMap<>();
		map.put("9527", new Student("华安",23));
		map.put("008", new Student("秋香",18));
		map.put("007", new Student("石榴",30));
		map.put("009", new Student("武状元",40));
		
		// 使用keySet遍历
		Set<String> keySet = map.keySet();
		for (String key : keySet) {
			// 根据键获得每一个学生对象
			Student stu = map.get(key);
			System.out.println(stu);
		}
	}
}
```

## 7. LinkedHashMap的特点

- LinkedHashMap继承HashMap，能够保证存取顺序一致。

```java
/**	
	LinkedHashMap基本使用
 */
public class MapDemo04 {
	public static void main(String[] args) {
		// 创建Map集合
		Map<String,String> map = new LinkedHashMap<String,String>();
		// 存储元素
		map.put("name", "张三");
		map.put("gender", "男");
		map.put("hobby", "撸代码");
		System.out.println(map);
	}
}
```

## 8. 方法的可变参数

- 可变参数的概述
  - 是JDK1.5新特性。
  - 前提：数据类型必须是确定的
  - 格式：数据类型...变量名， 参数个数可以是任意个。
  - 本质：就是一个数组	


- 示例代码

```java
public class VarArgDemo01 {
	public static void main(String[] args) {
//		System.out.println(sum());
		System.out.println(sum(1)); 
		System.out.println(sum(1,2));
		System.out.println(sum(1,2,3));
		System.out.println(sum(1,2,3,343,32213,1231231,3,13,1));
	}
	
	/**
	 * 使用可变参数定义方法
	 * @return
	 */
	public static int sum(int b,int...a) {
		// 定义求和变量
		int result = 0;
		for (int i : a) {
			result += i;
		}
		return result;
	}
	
	/**
	 * 求三个数之和
	 */
	/*public static int sum(int a,int b, int c) {
		return a + b + c;
	}*/
	
	/**
	 * 求a+b的和
	 */
	/*public static int sum(int a,int b) {
		return a + b;
	}*/
}
```

## 9. 可变参数的注意事项

-  一个方法参数列表中只能有一个可变参数而且必须是参数列表的最后一个。

## 10. Collections工具类常用方法

```java
* static boolean addAll(Collection c, T... elements) 
    * 将数组中的所有元素添加到指定的集合中
* static int binarySearch(List list, T key) 
    * 使用二分法在指定的集合中查找指定的的元素
    * 前提条件：集合中的元素必须是有序的
    * 如果找到，返回该元素在集合中的索引值。
    * 如果找不到，则返回值= -插入点-1
* static void sort(List<T> list) 
    * 对集合中的元素进行排序，默认是升序
* static void shuffle(List<?> list) 
   * 对集合中的元素进行乱序
```

- 示例代码

```java
public class CollectionsDemo01 {
	public static void main(String[] args) {
		// 创建集合
		ArrayList<String> list = new ArrayList<>();
		// 将数组中的所有元素添加到指定的集合中
		Collections.addAll(list, "a","abc","abc","xx","f");
		System.out.println(list);
		
		// 对list集合元素进行排序
		Collections.sort(list);
		System.out.println(list);
		
		// 打乱集合中元素的顺序
		Collections.shuffle(list);
		System.out.println(list);
		
		// 创建集合存储整数
		ArrayList<Integer> list01 = new ArrayList<>();
		Collections.addAll(list01, 1,4,5667,331,32143,6,10,34,56,89);
		
		// 对集合元素排序
		Collections.sort(list01);
		System.out.println(list01);
		// 查找指定元素在集合中的索引值
		int index = Collections.binarySearch(list01,100);
		System.out.println(index);
	}
}
```

## 11. Arrays工具类常用方法

```java
static String toString(int[] a) 
    * 将数组中元素拼接成指定格式的字符串。
static void sort(int[] a) 对数组的元素进行排序,默认是升序
static void	 fill(int[] a, int val) 
    * 使用指定的值填充数组中全部元素
static boolean equals(int[] a, int[] a2) 
    * 判断两个数组中的元素内容是否完全相同，个数，值都要相同返回true，返回false

static int binarySearch(int[] a, int key) 
    * 使用二分查找法在指定的数组中查找的元素

static List asList(T... a) 
    * 将数组的元素转换为集合
```

- 示例代码

```java
/**
 Arrays工具类的常用方法
 */
public class ArraysDemo01 {
	public static void main(String[] args) {
		// test01();
		// test02();
		// test03();
		// test04();
		// test05();
		
		test06();
	}
	
	/**
	 static List asList(T... a) 
		* 将数组的元素转换为集合
		* 返回的集合长度是固定的，不能增删元素。
	 */
	public static void test06(){
		Integer[] arr = {1,2,3,4,5,16,17,18,19,20}; 
		// 将数组转换为集合
		List<Integer> list = Arrays.asList(arr);
		System.out.println(list);
		
		String[] strs = {"a","b","c"};
		// List<String> list02 = Arrays.asList(strs);
		
		List<String> list02 = Arrays.asList("a","b","c");
		list02.add("d");
		System.out.println(list02);
	}
	
	/**
	static int binarySearch(int[] a, int key) 
		* 使用二分查找法(折半查找)在指定的数组中查找的元素
		* 使用前提：数组的元素必须是排好顺序的。
		* 如果找到了，则返回该元素在数组中索引值。
		* 如果没有找到，返回值=-插入点-1  
		* 
	 */
	public static void test05(){
		int[] arr = {1,2,3,4,5,16,17,18,19,20}; 
		Arrays.sort(arr);
		System.out.println(Arrays.binarySearch(arr,12));
	}
	
	/**
	 static boolean equals(int[] a, int[] a2) 
 		* 判断两个数组中的元素内容是否完全相同，个数，值都要相同返回true，返回false
	 */
	public static void test04(){
		int[] arr1 = {1,2,3,4};
		int[] arr2 = new int[3];
		arr2[0] = 1;
		arr2[1] = 2;
		arr2[2] = 3;
		// 判断两个数组是否相等
		System.out.println(Arrays.equals(arr1, arr2));
	}
	
	/**
	 static void	 fill(int[] a, int val) 
	 	* 使用指定的值填充数组中全部元素
	 */
	public static void test03() {
		int[] arr = new int[10];
		System.out.println(Arrays.toString(arr));
		Arrays.fill(arr, 10);
		System.out.println(Arrays.toString(arr));
	}
	
	/**
	 * static void sort(int[] a) 对数组的元素进行排序
	 * 默认是升序 
	 * 自定义比较器
	 */
	public static void test02() {
		// 定义数组
		int[] arr = {1,23,4,56,5,7};
		System.out.println("排序前：" + Arrays.toString(arr));
		// 对数组元素进行排序
		Arrays.sort(arr);
		System.out.println("排序后：" + Arrays.toString(arr));
	} 
	
	/**
	 * toString(): 将数组中元素拼接成指定格式的字符串。
	 */
	public static void test01() {
		// 定义数组
		int[] arr = {1,23,4,5,5,7};
		System.out.println(Arrays.toString(arr));
	}
}
```

## 12. 集合转数组
* 集合转数组使用的是ArrayList中的toArray()方法。

* 该方法是重载的方法
  ```java
  public Object[] toArray();
  public <T> T[] toArray(T[] a);
  ```

  * 如果参数数组足够放下集合中所有元素，则将集合中的元素添加到参数数组并将参数数组作为返回值。
  * 如果参数数组无法放下集合中所有元素,则参数数组只起到确定类型作用，方法逻辑会自动创建新数组存储集合内容,并返回新的数组。

* 示例代码

```java
/**
  ArrayList集合转数组的方法
 	  * Object[] toArray()：将集合中的元素存储到对象数组中并返回该对象数组。
 	  * T[]  toArray(T[] a) 
 	  	 * 如果传递的数组的长度小于集合中元素的个数，则该方法内部会创建一个
 	  	 	新的数组并将集合中的元素添加到新的数组中，并返回该数组。
 	  	 	此时传入的数组只起到一个作用：决定内部创建新数组的数据类型。
 	  	 * 如果传递数组的长度大于等于集合中元素的个数，则该方法内部会直接将集合中的
 	  	 	元素添加到传入的数组，并将该数组作为返回值。 	
 */
public class CollectionsDemo02 {
	public static void main(String[] args) {
		// 创建集合
		ArrayList<String> list = new ArrayList<>();
		Collections.addAll(list, "a","b","c");
		
		// 将集合list的元素转换为数组中
		Object[] objs = list.toArray();
		for (Object obj : objs) {
			// System.out.println(((String)obj).length());
		}
		// T[]  toArray(T[] a) 
		// 创建字符串数组
		String[] strs = new String[1];
		System.out.println(Arrays.toString(strs));
		// 将集合中的元素添加到指定的数组中
		String[] results = list.toArray(strs);
		System.out.println(strs == results);
		System.out.println(Arrays.toString(results));
		System.out.println(Arrays.toString(strs));
	}
}
```

## 13. 斗地主洗牌发牌案例完善

```java
/**
 11. 集合综合案例
	组牌
	洗牌
	发牌
	看牌
 */
public class DDZDemo {
	public static void main(String[] args) {
		// 创建集合用来存储
		Map<Integer,String> pokers = new HashMap<>();
		// 花色
		String[] colors = {"♦️","♣️","♥️","♠️"};
		// 数字
		String[] nums = {"3","4","5","6","7","8","9","10","J","Q","K","A","2"};
		// 定义一个牌的索引
		int index = 0;
		// 组牌
		for(String num:nums) {
			for(String color:colors) {
				// 拼接牌
				String poker = color.concat(num);
				// 将牌添加到pokers集合中
				pokers.put(index++, poker);
			}
		}
		// 添加大小王
		pokers.put(index++, "🃏");
		pokers.put(index, "🤴");
		System.out.println(pokers);
		
		// 牌的索引集合
		 ArrayList<Integer> indexes = new ArrayList<>(pokers.keySet());
		 System.out.println(indexes);
		// 洗牌
		Collections.shuffle(indexes);
		System.out.println(indexes);
		
		// 创建玩家集合
		ArrayList<Integer> player01 = new ArrayList<>();
		ArrayList<Integer> player02 = new ArrayList<>();
		ArrayList<Integer> player03 = new ArrayList<>();
		ArrayList<Integer> dp = new ArrayList<>();
		// 发牌
		for(index = 0;index < indexes.size(); index ++) {
			// 获得牌的索引
			int poker = indexes.get(index);
			if(index < 3) {
				dp.add(poker);
			} else if (index % 3 == 0) {
				player01.add(poker);
			} else if (index % 3 == 1) {
				player02.add(poker);
			}  else  {
				player03.add(poker);
			} 
		}
		/*System.out.println(player01);
		System.out.println(player02);
		System.out.println(player03);
		System.out.println(dp);*/
		
		// 看牌
		lookPoker("高进",player01, pokers);
		lookPoker("小刀",player02, pokers);
		lookPoker("星爷",player03, pokers);
		lookPoker("底牌",dp, pokers);
	}
	/**
	 * 查看玩家的牌
	 * @param player 玩家集合
	 * @param pokers 牌的集合
	 */
	public static void lookPoker(String name,ArrayList<Integer> player,Map<Integer,String> pokers) {
		System.out.print(name+":");
		// 对玩家集合进行排序
		Collections.sort(player);
		// 遍历玩家集合：键
		for (Integer key : player) {
			// 根据key获得牌
			String poker = pokers.get(key);
			System.out.print(poker + " ,");
		}
		System.out.println();
	}
}
```


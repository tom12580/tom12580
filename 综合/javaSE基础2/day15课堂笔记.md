# 知识点回顾

- 什么是进程
  - 正在运行中的程序
- 什么是线程
  - 进程中的一个独立的执行路径
- 线程的分类
  - 单线程：同一时间只能干一件事
  - 多线程：同一时间可以干多件事情。
- 线程和进程的作用
  - 线程：用来执行代码
  - 进程：用来封装线程并为线程分配资源
- 主线程的概念
  - 程序启动时系统自动创建的并执行main方法的线程。
  - 从main开始直到main方法结束
- 子线程的概念
  - 除了主线程以外的线程都称为子线程
  - 从run开始直到run方法结束。
- 线程的运行模式
  - 分时式模式
  - 抢占式模式：Java属性该中模式
- 多线程的优点
  - 提高CPU的使用率，提高程序的执行效率。
- 线程的常用方法

```Java
String getName();
void setName(String name);
static Thread currentThread();
static void sleep(long time);
```

- 线程的五种状态
  - 新建：刚刚new处来的时候，还没调用start方法
  - 就绪：调用了start方法，有执行资格，没有执行权
  - 运行：抢到了CPU，有执行权。
  - 阻塞：调用了sleep或wait方法
  - 死亡：任务正常执行完毕或调用了stop方法
- 线程安全的概念
  - 两个或多个线程在操作同一个共享资源时仍然能到正确的结果则称为线程安全。


- 实现线程安全的技术

  - 同步代码块：

    ```java
    synchronized(锁对象){
        
    }
    能够保证同一时间只有一个线程执行代码块中的代码。
    锁对象可以是任意类型的对象。
    锁对象必须被所有线程共享。
    ```

  - 同步方法

    ```java
    修饰符 synchronized 返回值类型 方法名(参数列表){
        
    }
    能够保证同一时间只有一个线程执行方法体中的代码。
    非静态同步方法锁对象：this
    静态同步方法锁对象是：类名.class
    ```

# 学习目标

- 能够独立使用Lock解决线程安全问题
- 能够使用Runnable实现方式自定义线程
- 能够说出死锁现象出现的根本原因
- 能够理解线程池的作用
- 能力独立完成利用Runnable接口向线程池提交任务
- 能够独立完成利用Callable接口向线程池提交任务
- 能够独立完成等待唤醒机制的案例

# 今日内容

- 实现线程安全的第三种方式
- 开启线程的其他方法


- 线程池
- 等待与唤醒机制
- 单例模式

## 1. Lock接口

```java
Lock接口概述
    * JDK1.5新特性。
    * 专门用来实现线程安全的技术

Lock接口常用实现类
    * ReentrantLock：互斥锁

Lock接口常用方法
    * void lock()  获取锁。
    * void unlock() 释放锁。

Lock使用注意事项
    * 获取锁和释放锁的代码必须成对出现。
```

- 卖票案例—使用Lock接口实现线程安全

```java
public class TicketThead extends Thread {
	// 定义变量记录总票数
	private static int tickets  = 100;
	// 创建互斥锁对象
	private static Lock lockObj = new ReentrantLock();
	
	@Override
	public void run() {
		while(true) {
			try {
				// 模拟休眠
				Thread.sleep(10);
				// 获取锁
				lockObj.lock();
				// 判断是否有剩余票数
				if(tickets > 0) {
					// 卖一张
					System.out.println(this.getName() + "  卖了一张票，还剩 "+(--tickets)+" 张");
					continue;
				} 
			} catch (InterruptedException e) {
				e.printStackTrace();
			} finally {
				// 释放锁
				lockObj.unlock();
			}
			System.out.println("票没有了...");
			break;
		}
	}
}

public class ThreadDemo01 {
	public static void main(String[] args) {
		// 创建两个线程：模拟两个卖票窗口
		TicketThead t1 = new TicketThead();
		TicketThead t2 = new TicketThead();
		
		// 设置线程名
		t1.setName("美女A");
		t2.setName("美女B");
		
		// 开启线程
		t1.start();
		t2.start();
	}
}
```

## 2. Runnable接口之实现线程

```java
实现线程的第二方式：Runnable接口
    * 创建一个类实现Runnable接口，重写run方法
    * 创建Runnable接口实现类对象，根据实现类对象创建Thread的对象
    * 调用start方法开启线程。

Thread类的构造方法
    * Thread(Runnable target) 
        * 根据target对象创建线程对象 
```

- 示例代码

```java
/**
 * Runnable接口实现类：用来封装线程任务。
 */
public class RunnableTask implements Runnable {
	private int tickets = 100;
	@Override
	public void run() {
	/*	while(true) {
			try {
				synchronized (this) {
					// 模拟休眠
					Thread.sleep(10);
					// 判断是否有剩余票数
					if(tickets > 0) {
						// 卖一张
						System.out.println(Thread.currentThread().getName() + "  卖了一张票，还剩 "+(--tickets)+" 张");
						continue;
					} 
				}
			} catch (InterruptedException e) {
				e.printStackTrace();
			} finally {
				
			}
			System.out.println("票没有了...");
			break;
		}*/
		
		while(tickets > 0) {
			saleTicket();
		}
		System.out.println("票没有了...");
	}
	
	// 锁对象：this
	public synchronized void saleTicket() {
		// 判断是否有剩余票数
		if(tickets > 0) {
			// 卖一张
			System.out.println(Thread.currentThread().getName() + "  卖了一张票，还剩 "+(--tickets)+" 张");
		} 
	}
}

public class ThreadDemo01 {
	public static void main(String[] args) {
		// 创建Runnable实现类对象
		RunnableTask target = new RunnableTask();
		// 创建Thread对象
		Thread t1 = new Thread(target);
		Thread t2 = new Thread(target);
		// 开启线程
		t1.start();
		t2.start();
	}
}
```

## 3. 实现Runnable接口的好处 

- 屏蔽了Java类单继承的局限性。
- 可以更好的实现在多个线程之间共享数据。


- 将线程与任务进行分离，降低的程序的耦合性。

```java
username
password
url
driverClass

配置文件
username
password
url
driverClass


class ThreadA extends Thread{
	/*
	public void run(){
		syso();
    }
    */
}

class ThreadB extends Thread{
/*
	public void run(){
		syso();
    }
    */
}

class RunnableTask implements Runnable{
    public void run(){
		syso();
    }
}

RunnableTask task = new RunnableTask();
new ThreadA(task).start();
new ThreadB(task).start();

```

## 4. 匿名内部类实现线程

```java
public class ThreadDemo02 {
	public static void main(String[] args) {
		// 使用匿名内部类：直接创建Thread类子类对象
		new Thread() {
			@Override
			public void run() {
				System.out.println("子线程 = " + this.getName());
			}
		}.start();
		
		// 使用匿名内部类：直接创建Runnable接口的实现类对象
		Runnable target = new Runnable() {
			public void run() {
				System.out.println("子线程 = " + Thread.currentThread().getName());
			}
		};
		new Thread(target).start();
		
		// 简化版
		new Thread(new Runnable() {
			public void run() {
				System.out.println("子线程 = " + Thread.currentThread().getName());
			}
		}).start();
	}
}
```

## 5. 线程池概述

```java
什么是线程池
    * 一个用来创建和管理线程的容器。

为什么要使用线程池
    * 频繁创建线程和销毁线程很耗CPU资源，为了提供线程的使用率，需要一种技术来保证
    * 线程执行完任何后不被销毁。而这种技术就是线程池技术。

线程池的思想
    * 线程复用

在JDK1.5之后，Java官方已经为程序猿提供了线程池，我们只需要创建对应的对象就可以使用线程池技术。

如何创建线程池对象
    * 通过工厂类Executors的如下静态方法创建
        * public static ExecutorService newFixedThreadPool(int nThreads);
        * nThreads：指定线程的个数

ExecutorService类概述
    * 该接口的实现类对象就是一个线程池对象。

ExecutorService类常用方法
    * Future<?> submit(Runnable task); 提交Runnable任务
    * Future<?> submit(Callable task); 提交Callable务
    *  void shutdown(); 销毁线程池，会等待线程池中的任何都执行完毕之后在销毁。
    *  void shutdownNow(); 立即销毁线程池，如果池中的任务还没就绪，就不会再执行了。
```

## 6. 提交Runnable任务

```java
public class ThreadPoolDemo {
	public static void main(String[] args) {
		// 创建线程池对象
		ExecutorService tp = Executors.newFixedThreadPool(2);
		// 提交任务
		tp.submit(new MyRunnableTask());
		tp.submit(new MyRunnableTask());
		tp.submit(new MyRunnableTask());
		
		// 销毁线程池
		 tp.shutdown();
		
		// 立即销毁线程池，如果池中的任务还没就绪，就不会再执行了。
		// tp.shutdownNow();
	}
}

/**
 * 自定义Runnable接口实现类
 */
class MyRunnableTask implements Runnable {
	@Override
	public void run() {
		try {
			Thread.sleep(1000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println("线程任务 = " + Thread.currentThread().getName());
	}	
}
```

## 7. 提交Callable任务

```java
/**
 * 线程池之提交Callable任务
 		* 定义一个类实现Callable接口，重写call方法编写线程要执行任务代码。
 		* 创建线程池对象
 		* 创建Callable接口实现类对象
 		* 调用线程池对象的submit方法传递实现类对象。
 		* 销毁线程池。
 */
public class ThreadPoolDemo02 {
	public static void main(String[] args) throws Exception {
		// 创建线程池对象
		ExecutorService tp = Executors.newFixedThreadPool(2);
		// 提交Callable任务
		Future<String> f = tp.submit(new MyCallableTask());
		// 获得返回值
		System.out.println(f.get());
		/*
		tp.submit(new MyCallableTask());
		tp.submit(new MyCallableTask());
		*/
		// 销毁线程池
		tp.shutdown();
	}
}

/**
 * 自定义Callable实现
 */
class MyCallableTask implements Callable<String> {

	@Override
	public String call() throws Exception {
		System.out.println("子线程任务 = " + Thread.currentThread().getName());
		return "abc";
	}
}
```

## 8. 线程池练习
* 调用线程池对象方法提交线程任务，同时执行下面任务。
  * 计算1到100的和
  * 计算1到200的和
* 示例代码

```java
/**
 调用线程池对象方法提交线程任务，同时执行下面任务。
	- 计算1到100的和
	- 计算1到200的和
 */
public class ThreadPoolDemo03 {
	public static void main(String[] args) throws Exception{
		// 创建线程池对象
		ExecutorService tp = Executors.newFixedThreadPool(2);
		// 提交任务
		Future<Integer> f1 = tp.submit(new CallableTask(5));
		Future<Integer> f2 = tp.submit(new CallableTask(5));
		
		// Future接口的get()方法：是一个同步方法  (异步是多线程的代名词)
		System.out.println(f1.get());
		System.out.println(f2.get());
		
		int sum = f1.get() + f2.get();
		System.out.println(sum);
		// 销毁线程池
		tp.shutdown();
		for (int index = 0; index < 10; index++) {
			System.out.println("main = " + index);
		}
	}
}
/**
 * 自定义Callable接口实现类
 * @author pkxing
 *
 */
class CallableTask implements Callable<Integer> {
	// 定义一个成员变量
	private int num;
	public CallableTask(int num) {
		this.num = num;
	}
	@Override
	public Integer call() throws Exception {
		
		int sum = 0;
		for (int i = 1; i <= num; i++) {
			System.out.println("call = " + i);
			sum += i;
		}
		Thread.sleep(2000);
		return sum;
	}
}
```

## 9. 线程死锁概述(了解)

- 线程死锁的概念
  - 指两个或两个以上的线程在执行过程中因争夺资源而造成的一种相互等待的现象。


- 死锁产生的四个必要条件

  - 互斥条件：一个资源同一时间只能有一个线程访问。
  - 请求与保持条件：一个线程请求资源受阻，但对已经请求到的资源又保持不放。
  - 不可剥夺条件：一个线程在未使用该资源时，不能强行剥夺其释放资源。
  - 循环等待条件：循环等待对方获取的资源。

  ![](线程死锁图解.png)

## 10. 线程死锁代码实现

```java
public class MyRunnable implements Runnable {
	// 创建锁对象
	private Object lockA = new Object();
	private Object lockB = new Object();
	
	int index = 0;
	
	@Override
	public void run() {
		while(true) {
			if(index % 2 ==0 ) {
				synchronized (lockA) {
					System.out.println(Thread.currentThread().getName() + "...if...lockA...");
					// 美女B
					synchronized (lockB) {
						System.out.println(Thread.currentThread().getName() + "...if...lockB...");
					}
				}
			} else {
				synchronized (lockB) {
					System.out.println(Thread.currentThread().getName() + "...else...lockB...");
					// 美女A						
					synchronized (lockA) {
						System.out.println(Thread.currentThread().getName() + "...else...lockA...");
					}
				}
			}
			index ++;
		}
	}
}
```

## 11. 线程等待与唤醒介绍

- 线程等待与唤醒机制概述

  - 又称为线程间通讯。

- 线程等待与唤醒机制相关方法

  ```java
  void wait(); 等待 让当前正在执行的线程释放CPU的使用权，进入等待。
  void notify(); 唤醒  随机唤醒一个正在等待的线程
  void notifyAll(); 唤醒所有正在等待的线程。

  注意事项
  	* 以上三个方法必须由锁对象调用.
  	* 以上三个方法必须在同步代码块或同步方法中调用
  ```

## 12. 线程等待与唤醒案例
* 定义一个资源共享类。
  * 属性：姓名和性别
* 开启两个线程：一个输入线程，一个输出线程。
  * 输入线程的任务是给共享对象属性赋值。
  * 输出线程的任务是获取共享对象属性的值，输出在控制台。
* 要求
  * 输入线程和输出线程要交替执行。
* 示例代码
```java
/**
 * 资源类
 * @author pkxing
 *
 */
public class Resource {
	public String name;
	public String gender;
}

/**
 * 输入线程
 * @author pkxing
 *
 */
public class InputThread extends Thread {
	// 资源对象
	private Resource r;
	public InputThread(Resource r) {
		this.r = r;
	}

	int index = 0;
	@Override
	public void run() {
		while(true) {
			synchronized (r) {
				// 给对象r的成员变量赋值
				if(index % 2 == 0) {
					r.name = "张三";
					r.gender = "男";
				} else {
					r.name = "lisi";
					r.gender = "nv";
				}
				
				try {
					// 唤醒输出线程
					r.notify();
					// 让当前线程进入等待
					r.wait();
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
			// 索引加一
			index ++;
		}
	}
}

/**
 * 输出线程
 * @author pkxing
 *
 */
public class OutputThread extends Thread {
	// 资源对象
	private Resource r;
	public OutputThread(Resource r) {
		this.r = r;
	}

	@Override
	public void run() {
		while(true) {
			synchronized (r) {
				// 判断输入线程是否已经输入了数据，如果没有则当前线程进入等待
				if(r.name == null) {
					try {
						r.wait();
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				}
				// 直接输出成员变量的值到控制台
				System.out.println(r.name+"="+r.gender);
				try {
					// 先唤醒输入线程
					r.notify();
					// 让当前线程进入等待
					r.wait();
					
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
		}
	}
}

/**
 等待与唤醒机制案例
 	
	java.lang.IllegalMonitorStateException
	线程等待与唤醒机制相关方法
		void wait(); 等待 让当前正在执行的线程释放CPU的使用权，进入等待。
		void notify(); 唤醒  随机唤醒一个正在等待的线程
		void notifyAll(); 唤醒所有正在等待的线程。
	注意事项
		* 以上三个方法必须由锁对象调用.
		* 以上三个方法必须在同步代码块或同步方法中调用
 */
public class TheadDemo {
	public static void main(String[] args) {
		// 创建资源对象
		Resource r = new Resource();
		// 创建输入线程和输出线程对象
		InputThread tin = new InputThread(r);
		OutputThread tout = new OutputThread(r);
		// 开启线程
		tin.start();
		tout.start();
	}
}
```

## 13. 单例设计模式

- 单例模式概述
  - 类从程序运行开始直到程序运行结束只有一个对象。
  - 单例类必须有私有构造方法。
  - 单例类必须自己创建自己的对象：唯一的一个对象。
  - 单例类要提供静态方法供外界访问对象


- 单例实现方式
  - 懒汉式
    - 当外界需要使用对象时，如果该对象还没有创建，就创建该对象。
    - 线程不安全
  - 饿汉式
    - 当类加载时就创建该单例对象，不管外键是否会使用到。
    - 线程安全
- 示例代码

```java
/**
 * 单例类
 */
public class Singleton {
	//----------- 饿汉式 -------------------
	// 定义成员变量
	//private static Singleton s = new Singleton();
	// 私有构造方法
	// private Singleton() {}
	
	// 提供静态方法供外界访问该单例对象
	/*
	public static Singleton getInstance() {
		return s;
	}*/
	// --------- 懒汉式 ---------------------
	// 定义成员变量
	private static Singleton s = null;
	// 私有构造方法
	private Singleton() {}
	// 提供静态方法供外界访问该单例对象
	// 使用静态同步方法实现线程安全:不推荐使用
	public static synchronized Singleton getInstance() {
		// 判断s是否已经创建
		if(s == null) {
			// 创建s对象
			s = new Singleton();
		}
		// 返回单例对象s
		return s;
	}
	
	// 使用同步代码块实现线程安全：推荐使用
	public static Singleton getInstance02() {
		if(s == null) {
			// A,B
			synchronized (Singleton.class) {
				// 判断s是否已经创建
				if(s == null) {
					// 创建s对象
					s = new Singleton();
				}
			}
		}
		// 返回单例对象s
		return s;
	}
}
```

## 14. 常见面试题

1. Runnable和Callable有什么不同？
   - Runnable接口是JDK1.0出现，Callable接口是JDK1.5出现
   - Runnable接口中run方法没有返回值，不能声明抛出异常。
   - Callable接口中call方法有返回值，可以声明抛出异常。
2. sleep()和wait()的区别
   - sleep需要指定休眠时间，wait不需要指定时间
   - sleep休眠到指定的时间会自动醒来，wait需要被唤醒
   - sleep在休眠时不会释放锁对象，wait方法会释放锁对象
   - sleep方法可以在任意类的任意方法中使用
   - wait方法只能在同步方法或同步代码块中使用并且只能通过锁对象调用。
3. Lock接口和synchronized区别
   - synchronized是一个关键字，Lock是一个接口
   - synchronized代码块或方法体执行完毕后会自定释放锁对象，Lock接口在使用时必须手动调用方法释放锁对象。
   - synchronized中出现异常时会自动释放锁对象，Lock接口使用时出异常也不会自动释放锁，需要在finally代码块中释放锁对象。
   - 当资源竞争不激烈时，使用哪个一个实现线程安全效率都差不多，如果资源竞争很激烈时，Lock接口的效率会远远高于synchronized的效率。


​		
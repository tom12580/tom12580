# 学习目标
* 能够说出进程与线程的作用
* 能够理解多线程提高资源利用率的原因
* 能够说出一个进程中多个线程的执行过程
* 能够独立使用Thread编写线程类
* 能够独立写出卖票的案例
* 能够独立解决线程安全问题
* 能够阐述线程的生命周期状态

# 今日内容

- 多线程
- 线程安全

# 第1章 多线程

## 1.1 进程和线程概述

```java
线程的引入
	* 要同时下载文件A和文件B，如何实现？

什么是进程？
	* 正在运行中的程序就是一个进程。
	
什么是线程？
	* 线程是进程中的一个独立执行路径。

线程的分类
	* 单线程：进程中只有一个独立的执行路径，同一时间只能干一件事情。
	* 多线程：进程中有多个独立的执行路径，同一时间可以干多件事情。

线程和进程的作用
	* 进程：用来封装线程，为线程执行任务分配资源。
	* 线程：用来执行代码的

进程和线程的举例
	* 工厂与工人：工厂可以理解为进程，工人可以理解为线程。
	* 公路和车道：公路可以理解为进程，车道可以理解为线程
```

## 1.2 开启线程的方式

### 1.2.1 创建和启动线程的方式

```java
* 自定义一个类继承Thread类
* 重写run方法：将线程任务相关的代码写在该方法中。
* 创建子类对象，调用start方法开启线程
```

### 1.2.2 示例代码

```java
/**
 * 自定义线程
 */
public class MyThread extends Thread {
	@Override
	public void run() {
		for (int i = 0; i < 10; i++) {
			System.out.println("run = " + i);
		}
	}
}

public class ThreadDemo02 {
	public static void main(String[] args) {
		// 创建线程对象
		MyThread t = new MyThread();
		// 直接调用run方法：不能开启新的执行路径，当前线程在干活
		// t.run();
		// 开启线程：会开启一个新的执行路径，在新的执行路径中执行run方法中的代码
		t.start();
		for (int i = 0; i < 10; i++) {
			System.out.println("main = " + i);
		}
	}
}
```

## 1.3 主线程和子线程的概念

```java
* 什么是主线程
    * 程序启动过程中自动创建并执行main方法的线程就是主线程。
* 主线程的执行路径
    * 从main方法开始直到main方法结束。

* 什么是子线程
    * 除了主线程以为的其他线程都称为子线程。

* 子线程的执行路径
    * 从run方法开始直到run方法结束。
```

## 1.4 线程的运行模式

- 分时式模式
  - 每一个线程平均分配CPU的使用权。
- 抢占式模式
  - 每一个线程一起去抢CPU的使用权，哪个线程抢到CPU则执行哪个线程的任务。
  - 优先级高的线程使用CPU的概率高，优先级低的线程使用CPU的概率低。
  - Java线程运行模式允许该种模式。

## 1.5 迅雷的多线程下载[了解]

## 1.6 多线程的优缺点

- 提高了CPU的使用率


- 提高了代码的执行效率

![](/Users/pkxing/Documents/82期就业班/day14/笔记/多线程的优缺点.png)

## 1.7 多线程内存图解

- 每一个线程都有自己独立的栈空间
- 每个线程中的代码都是从上往下按顺序执行

![](/Users/pkxing/Documents/82期就业班/day14/笔记/多线程内存图解.png)

## 1.8 线程常用方法 

```java
* String getName(); 获得线程名称,默认是Thread-序号
* void setName(String name); 设置线程名称
* static Thread	currentThread();获得执行当前方法所在的线程对象。
* static void sleep(long millis); 让当前线程休眠指定的毫秒值
```

- 示例代码

```java
public class ThreadDemo04 {
	public static void main(String[] args) {
		// 创建线程对象
		MyThread t = new MyThread();
		
		System.out.println(t.hashCode());
		// 设置线程名称
		t.setName("下载文件A线程");
		// 获得线程名称
		System.out.println(t.getName());
		
		// 获得主线程名称
		Thread mt =  Thread.currentThread();
		System.out.println(mt.getName());
		// 开启线程
		t.start();
		try {
			// 让线程休眠2秒
			Thread.sleep(2000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		for (int index = 0; index < 10; index++) {
			System.out.println("main = " + index);
		}
	}
}
```

## 1.9 线程的状态(背熟)

- 新建：刚刚创建完毕，还没有调用start方法
- 就绪：调用了start方法，有执行资格，没有CPU使用权
- 运行：抢到了CPU的使用权
- 阻塞：调用了sleep或wait方法
- 死亡：任务正常执行完毕或调用了stop方法

![](线程的状态图.png)

# 第2章 线程安全

## 2.1 线程安全概述

```java
什么是线程安全
	* 两个或多个线程同时操作一个共享资源时，仍然能得到正确的结果，则称为线程安全。

线程安全的案例
	* 火车站卖票的案例

卖票逻辑
	* 定义一个变量用来记录总票数
	* 判断是否有剩余票数
		* 有则卖一张，票数减一。
		* 没有则提示用户票没了	
```

- 卖票案例代码实现—存在线程安全问题

```java
/**
 * 卖票线程
 * @author pkxing
 *
 */
public class TicketThead extends Thread {
	// 定义变量记录总票数
	private static int tickets  = 100;
	
	@Override
	public void run() {
		while(true) {
			// 判断是否有剩余票数
			if(tickets > 0) {
				try {
					// 模拟休眠
					Thread.sleep(100);
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
				// 卖一张
				System.out.println(this.getName() + "  卖了一张票，还剩 "+(--tickets)+" 张");
			} else {
				System.out.println("票没有了...");
				break;
			}
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

## 2.2 实现线程安全的方式

### 2.2.1 同步代码块实现线程安全

```java
/**
	同步代码块解决线程不安全问题
		
	同步代码块的格式
		synchronized(锁对象){
			// 操作共享资源的代码
		}
	同步代码块的原理
		* 同步代码块能够保证同一时间只有一个线程执行代码块中的代码。
	
	锁的概述
		* 任意类型的对象都可以充当锁。 
		* 锁对象必须被所有线程共享(共用一把锁)
 */
public class TicketThead extends Thread {
	// 定义变量记录总票数
	private static int tickets  = 100;
	// 定义锁对象
	private static Object lockObj = new Object();
	@Override
	public void run() {
			while(true) {
				// 同步代码块实现线程安全
				synchronized (lockObj) {
					// 判断是否有剩余票数
					if(tickets > 0) {
						try {
							// 美女A
							// 模拟休眠
							Thread.sleep(10);
						} catch (InterruptedException e) {
							e.printStackTrace();
						}
						// 卖一张
						System.out.println(this.getName() + "  卖了一张票，还剩 "+(--tickets)+" 张");
						continue;
				}
			}
			System.out.println("票没有了...");
			break;
		}
	}
}
```

### 2.2.2 同步方法实现线程安全

```java
/**
	同步方法解决线程不安全问题
	
	
	同步方法的格式
		* 修饰符 synchronized 返回值类型 方法名(参数) {}
	
	同步方法的原理
		* 同一时间只能有一个线程执行同步方法中的代码
	
	非静态同步方法的锁对象默认是：this
	静态同步方法的锁对象默认是：类名.class
	
 */
public class TicketThead extends Thread {
	// 定义变量记录总票数
	private static int tickets  = 100;
	
	@Override
	public  void run() {
		while(tickets > 0) {
			saleTicket();
		}
		System.out.println("票没了");
	}
	
	/**
	 * 静态同步方法锁对象：类名.class
	 * 美女A  
	 * 美女B  
	 */
	public static synchronized void saleTicket() {
		if(tickets > 0 ) {
			try {
				// 模拟休眠
				Thread.sleep(10);
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			// 卖一张
			System.out.println(Thread.currentThread().getName() + "  卖了一张票，还剩 "+(--tickets)+" 张");
		}
	}
}
```

## 2.3 常见面试题

- start和run方法的区别
  - start方法会开启新的线程，在新的执行路径中执行run方法，start方法中会触发run方法的调用。
  - run方法不会开启新的线程，在当前线程中执行run方法。
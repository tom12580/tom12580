# 知识点回顾

## 1. 创建线程方式

```java
1. 创建子类继承Thread类
	创建子类继承Thread类，重写run方法，将任务相关的代码写在run方法中。
	创建子类对象，调用start方法开启线程。
	
2. 创建类实现Runnable接口
	创建类实现Runnable接口，重写run方法，将任务相关的代码写在run方法中。
	创建实现类对象，根据实现类对象创建Thread类对象，调用start方法开启线程。
	
3. 匿名内部类
	直接创建Thread类的子类对象：
    new Thread(){ public void run(){
    	// 任务代码
    }}.start();
	
	直接创建Runnable接口的实现类对象
    Runnable r = new Runnable(){
   	 	public void run(){
    		// 任务代码
    	}
    }
    new Thread(r).start();
	
4. 线程池
	通过Executors类的静态方法创建线程池对象：ExecutorService newFixedThreadPool(int size);
	调用线程池对象的submit方法提交任务：
		提交Runnable任务：submit(Runnable r)
         提交Callable任务：submit(Callable c)
     调用线程池对象的shutdown方法关闭线程池。
```

## 2. 实现线程安全方式

```java
1. synchronized关键字
	同步代码块：synchronized(锁对象){}
	同步方法：修饰符 synchronized 返回值类型 方法名(参数列表) {}
		非静态同步方法锁对象是：this
		静态同步方法锁对象是：类名.class
     保证锁对象必须被所有线程共享(锁对象唯一)
2. Lock接口
	创建接口实现类对象：ReentrantLock lock = new ReentrantLock();
	void lock(); 获取锁
	void unlock();释放锁对象
	获取锁和释放锁的代码要成对出现。
```

## 3. 线程的五种状态

```JAVA
新建
就绪
运行
阻塞
死亡
```

## 4. 等待与唤醒机制相关的方法

```java
void wait(); 让当前线程释放CPU使用权进入等待状态，会释放锁对象。
void notify();随机唤醒一个处于等待的线程，让其进入就绪状态。
void notifyAll();唤醒所有正在等待的线程，让它们一起进入就绪状态。
	以上三个方法必须在同步方法或同步代码块中使用，且必须通过锁对象调用。
```

# 学习目标

* 能够辨别UDP和TCP协议特点
* 能够说出UDP协议下两个常用类名称		
* 能够说出TCP协议下两个常用类名称
* 能够编写UDP协议下字符串数据传输程序
* 能够编写TCP协议下字符串数据传输程序
* 能够TCP协议下文件上传案例

# 今日内容

- 网络编程

## 1. 网络编程概述

- 网络编程的作用
  - 用来解决计算机与计算机之间数据传输的问题。


- 网络编程的三要素

  ![](/Users/pkxing/Documents/82期就业班/day16/笔记/网络通讯三要素.png)



## 2. IP地址类之InetAddress

```java
InetAddress类概述
    * 一个该类对象就表示一个IP对象。
    * 比如："192.168.87.100" ==> InetAddress对象

InetAddress类构造方法
    * static InetAddress	 getLocalHost(): 获得本地主机ip地址对象。
    * static InetAddress	 getByName(String host) 
        * 根据ip地址字符串或域名或主机名获得ip地址对象。

InetAddress类成员方法
    *  String getHostName(); 获得主机名
    *  String getHostAddress(); 获得IP地址字符串
```

- 示例代码

```java
public class InetAddressDemo {
	public static void main(String[] args) throws Exception {
		// 获得本地主机的ip地址对象
		InetAddress inet = InetAddress.getLocalHost();
		// pkxingdeMacBook-Pro.local/10.211.55.2
		// 主机名/ip地址字符串
		System.out.println(inet);
		System.out.println(inet.getHostName()); // pkxingdeMacBook-Pro.local
		System.out.println(inet.getHostAddress()); // 10.211.55.2
		
		// 根据ip地址获得ip地址对象
		InetAddress inet02 = InetAddress.getByName("pkxingdeMacBook-Pro.local");
		System.out.println(inet02);
	}
}
```

## 3. UDP协议

UDP ==> User Datagram Protocol   用户数据报(包)协议

### 3.1 特点

- 面向无连接的协议
- 发送端只管发送信息，不确认对方是否存在，接收端接收到消息也不会反馈消息给发送端。
- 发送数据大小限制在64k以内。
- 基于数据包传输数据：将要发送的内容，源和目的地封装到一个数据包，将数据包发送出去。
- 因为是面向无连接，速度快但不可靠。

### 3.2 应用场景

- 即时通讯(QQ，飞秋)
- 在线视频
- 网络语音电话

### 3.3 与UDP通信相关的类

- DatagramSocket
  - 作用：发送对象：用来负责发送和接收数据包。
  - 比喻：码头
- DatagramPacket
  - 作用：数据包对象：用来封装要发送的数据和要接收的数据。
  - 比喻：集装箱

```java
DatagramPacket类概述
    * 该类表示数据报包。

DatagramPacket类构造方法
    * DatagramPacket(byte[] buf, int length, InetAddress address, int port) 
        * 创建发送端的数据包
        * buf：封装要发送的内容
        * length：发送内容的长度 单位：字节
        * address： 接收端的ip地址对象
        * port：端口号

    * DatagramPacket(byte[] buf, int length) 
        * 创建接收端的数据包
            * buf：用来存储接收到的内容
            * length：能够接收的数据长度  
  
DatagramPacket类成员方法
		* int getLength(); 获得实际接收到的字节数。
		* int getPort(); 获得发送端或接收端的端口号
		* InetAddress getAddress(); 获得发送端的IP地址对象。

DatagramSocket类概述
    * 用来发送和接收数据报包的套接字。

DatagramSocket类构造方法
    * DatagramSocket(); 创建发送端的Socket对象
    * DatagramSocket(int port); 创建接收端的Socket对象

DatagramSocket类成员方法
    * void send(DatagramPacket p)： 发送数据包
    * void receive(DatagramPacket p)：接收数据包
```

## 5. UDP发送端代码实现

```java
public class UDPSender {
	public static void main(String[] args) throws Exception {
		// 要发送的内容
		byte[] msg = "约吗".getBytes();
		// 创建数据包对象:类似集合装箱
		DatagramPacket dp = new DatagramPacket(msg, msg.length, InetAddress.getLocalHost(), 6666);
		// 创建发送对象：DatagramSocket 发送数据包
		DatagramSocket ds = new DatagramSocket();
		// 发送数据包
		ds.send(dp);
		// 关闭资源：释放端口号
		ds.close();
	}
}
```

## 6. UDP接收端代码实现

```java
public class UDPReceive {
	public static void main(String[] args) throws Exception {
		// 创建DatagramSocket对象：用来负责接收发送端发送的数据包
		DatagramSocket ds = new DatagramSocket(6666);
		// 创建字节数组：用来存储接收到的实际内容：约吗
		byte[] buf = new byte[1024];
		// 创建数据包对象：用来存储接收到的数据
		DatagramPacket dp = new DatagramPacket(buf, buf.length);
		// 调用接收方法接收数据包
		ds.receive(dp);
		System.out.println("come here");
		// 关闭资源释放端口号
		ds.close();
	}
}
```

## 7. UDP接收端的拆包

```java
public class UDPReceive {
	public static void main(String[] args) throws Exception {
		// 创建DatagramSocket对象：用来负责接收发送端发送的数据包
		DatagramSocket ds = new DatagramSocket(6666);
		// 创建字节数组：用来存储接收到的实际内容：约吗
		byte[] buf = new byte[1024];
		// 创建数据包对象：用来存储接收到的数据
		DatagramPacket dp = new DatagramPacket(buf, buf.length);
		// 调用接收方法接收数据包
		ds.receive(dp);
		
		// 获得实际接收到的字节个数
		int length = dp.getLength();
		System.out.println(length);
		// 将字节数组转换为字符串输出
		System.out.println(new String(buf,0,length));
		
		// 获得发送端的ip地址
		String ip = dp.getAddress().getHostAddress();
		System.out.println(ip);
		
		// 获得发送端的端口号
		int port = dp.getPort();
		System.out.println(port);
		System.out.println("come here");
		// 关闭资源释放端口号
		ds.close();
	}
}
```

## 8. 给feiQ发消息
* 示例需求
  * 给自己电脑的feiQ发信息。
  * 给全部同学群发feiQ发信息。

* 给飞秋发送消息的格式

  * **version:time :sender : ip: flag:content**

  ```Java
  version(版本):  1.0  必须是1.0
  time：	时间 long类型
  sender :	发送者  
  ip:		IP地址或主机名都可以
  flag: 标识符。 如果是聊天的标识符是 32 。
  content	才是你真正要发送的数据。
  最后没有冒号，严格注意格式，不然没有效果。
  ```

- 示例代码

```java
/**
 * 发消息给飞秋
 * 	默认的端口号：2425
 */
public class SendMsgToFeiQ {
	public static void main(String[] args) throws Exception{
		// System.out.println(InetAddress.getLocalHost());
		// 要发送的内容
		byte[] content = getContent("好好学习，迎娶班主任").getBytes();
		// 创建数据包对象：用来封装要发送的数据
		DatagramPacket dp = new DatagramPacket(content, content.length, InetAddress.getByName("192.168.87.94"), 2425);
		// 创建Socket对象：用来发送数据包
		DatagramSocket ds = new DatagramSocket();
		// 发送数据包
		ds.send(dp);
		// 关闭资源释放端口号
		ds.close();
	}
	
	/**
	 给飞秋发送消息的格式
		version:time :sender:ip: flag:content
	
		version：版本号  必须1.0  
		time: 发送时间 当前时间的毫秒值
		sender: 发送者项目
		ip: 发送端的ip地址  
		flag: 标志位 发送文本消息是：32
		content：要送的真实数据
	 */
	public static String getContent(String content) {
		// 创建可变字符串
		StringBuilder sb  = new StringBuilder();
		sb.append("1.0:");
		sb.append(System.currentTimeMillis()+":");
		sb.append("习总:");
		sb.append("192.168.87.100:");
		sb.append("32:");
		sb.append(content);
		return sb.toString();
	}
}
```

## 10. TCP协议

### 10.1 特点

- 面向连接的协议
- 通过三次握手建立连接，连接成功后建立数据传输通道。
- 通过四次挥手断开连接，保证数据能够完整的到达对方。
- 传输数据大小没有限制。
- 基于IO流进行数据传输。
- 因为是面向连接，速度慢但是可靠的协议。

![](/Users/pkxing/Documents/82期就业班/day16/笔记/TCP三次握手和四次挥手图解.png)

### 10.2 应用场景

- 文件传输：上传和下载
- 发送邮件
- 远程登录

### 10.3 与TCP通信相关的类

- Socket：客户端Socket，该类的对象就代表一个客户端。
- ServerSocket：服务器端Socket，该类的对象就代表一个服务器端。

## 11. TCP通信之客户端

```java
Socket类概述
    * 一个该类对象代表一个客户端程序。

Socket类构造方法
    * Socket(String host, int port) 
        * 根据服务器端地址和端口号创建客户端Socket对象 
        * 一旦执行该方法创建客户端Socket对象，就会立即连接指定的服务端，
            如果服务器没有开启就直接抛异常。
        * 如果没有出现异常，则代表建立连接成功。

Socket类成员方法
    * OutputStream getOutputStream(); 
        * 获得字节输出流，通过该输出流输出数据就是输出到服务器端。
    * InputStream getInputStream();
        * 获得字节输入流，通过该输入流就可以读取服务器端返回的数据。

TCP客户端代码实现步骤
    * 创建Socket对象并指定服务器的IP地址和端口号。
    * 调用getOutputStream方法获得字节输出流对象
    * 通过字节输出流对象往服务器端发送数据
    * 调用getInputStream方法获得字节输入流对象
    * 通过字节输入流对象读取服务器端返回的数据
    * 关闭Socket对象。
```

- 示例代码

```java
public class TCPClient {
	public static void main(String[] args) throws Exception{
		// 创建Socket对象
		Socket socket = new Socket("127.0.0.1",8888);
		// 获得字节输出流
		OutputStream out = socket.getOutputStream();
		// 向服务器端输出数据
		out.write("约吗? 服务器端".getBytes());
		// 获得字节输入流
		InputStream in = socket.getInputStream();
		// 调用方法读取服务器端返回的数据
		// 创建字节数组
		byte[] buf = new byte[1024];
		int len = in.read(buf);
		System.out.println("len = " + len);
		System.out.println(new String(buf,0,len));
		
		// 关闭资源释放端口号
		socket.close();
	}
}
```

## 12. TCP通信之服务器端

```java
ServerSocket类概述
    * 创建该类的对象就表示开启了一个服务器端程序。

ServerSocket类构造方法
    * ServerSocket(int port)：创建服务器端Socket并指定端口号。

ServerSocket类成员方法
    * Socket accept(); 等待客户端连接并获得与客户端相关的Socket对象

TCP服务器端代码实现步骤
    * 创建ServerSocket对象并指端口号。
    * 调用accept方法等待客户端连接并获得客户端相关的Socket对象
    * 调用Socket对象的getInputStream方法获得字节输入流对象
    * 通过字节输入流对象读取客户端发送的数据
    * 调用Socket对象的getOutputStream方法获得字节输出流对象
    * 通过字节输出流对象往客户端发送数据
    * 关闭Socket和ServerSocket对象。
```

- 示例代码

```java
public class TCPServer {
	public static void main(String[] args)throws Exception {
		// 创建服务器Socket对象
		ServerSocket serverSocket = new ServerSocket(8888);
		// 等待客户端连接并获得与客户端相关的Socket对象
		Socket socket = serverSocket.accept();
		
		
		// 获得字节输入流，通过该输入流对象就可以读取客户端发送的数据
		InputStream in = socket.getInputStream();
		// 创建字节数组
		byte[] buf = new byte[1024];
		int len = in.read(buf);
		System.out.println("len = " + len);
		System.out.println(new String(buf,0,len));
		
		// 获得字节输出流对象，通过输出流对象输出数据到客户端
		OutputStream out = socket.getOutputStream();
		out.write("约，8点不见不散".getBytes());
		
		
		// 关闭服务器
		serverSocket.close();
		socket.close();
	}
}
```

## 13. TCP文件上传案例

- 需求：将文件上传到指定IP的主机
  - TCP的服务端可以接受多个客户端的连接
  - TCP的客户端上传文件到服务器端
  - 文件保存到c:/java/file文件下，文件名随机生成，只要不出现文件覆盖即可。
  - 服务器端需要反馈上传状态(成功或失败)给客户端。


- 示例代码

```java

/**
 * 上传文件的客户端代码实现
 * @author pkxing
 *
 */
public class UploadTCPClient {
	public static void main(String[] args) throws Exception{
		// 创建Socket对象
		Socket socket = new Socket("192.168.87.95",9999);
		// 创建字节输出流对象
		FileInputStream fis = new FileInputStream("/Users/pkxing/documents/aaa.png");
		// 获得字节输出流对象：用来发送文件数据
		OutputStream out = socket.getOutputStream();
		
		// 定义一个字节数组
		byte[] buf = new byte[1024];
		// 定义一个变量接收实际读取到字节个数
		int len = -1;
		while((len = fis.read(buf)) != -1) {
			// 利用out将数据输出到服务器中
			out.write(buf, 0, len);
		}
		
		// 关闭输出流：往服务器端输出一个结束标记
		socket.shutdownOutput();
		
		System.out.println("客户端......");
		// 获得字节输入流对象
		InputStream in = socket.getInputStream();
		// 读取服务器端返回的状态
		len = in.read(buf);
		System.out.println(new String(buf,0,len));
		// 关闭流
		fis.close();
		// 关闭socket
		socket.close();
	}
}


/**
需求：将文件上传到指定IP的主机
- TCP的服务端可以接受多个客户端的连接
- TCP的客户端上传文件到服务器端
	- 文件保存到c:/java/file文件下，文件名随机生成，只要不出现文件覆盖即可。
	- 服务器端需要反馈上传状态(成功或失败)给客户端。
 */
public class UploadTCPServer {
	public static void main(String[] args) throws Exception{
		// 创建服务器端Socket对象
		ServerSocket serverSocket = new ServerSocket(9999);
		// 使用循环等待客户端连接
		while(true) {
			// 客户端连接并获得与客户端相关的Socket
			Socket socket = serverSocket.accept();
			// 创建一个线程为客户端服务
			new UploadThread(socket).start();
		}
	}
}


/**
 * 上传线程
 * @author pkxing
 *
 */
public class UploadThread extends Thread {
	// 成员变量：接收客户端Socket
	private Socket socket ;
	public UploadThread(Socket socket) {
		super();
		this.socket = socket;
	}
	@Override
	public void run() {
		FileOutputStream fos = null;
		try {
			// 获得字节输入流对象：读取客户端发送的文件数据
			InputStream in = socket.getInputStream();
			// 获得当前系统毫秒值
			int index = new Random().nextInt(99999999);
			String fileName = System.currentTimeMillis()+""+index+".png";
			// 创建字节输出流：关联目标文件
			fos = new FileOutputStream(new File("/Users/pkxing/documents/bbb",fileName));
			// 定义字节数据存储读取到数据
			byte[] buf = new byte[1024];
			int len = -1;
			// 循环读写数据
			while((len = in.read(buf)) != -1) {
				// 将数据输出到fos关联目标文件
				fos.write(buf, 0, len);
			}
			
			// 输出客端户的ip地址信息
			String clientIP = socket.getInetAddress().getHostAddress();
			System.out.println("接收到ip为：" +  clientIP +" 用户上传的文件");
			// 返回一个状态给客户端
			// 获得字节输出流对象
			OutputStream out = socket.getOutputStream();
			out.write("上传成功".getBytes());
		}  catch (Exception e) {
			try {
				// 获得字节输出流对象
				OutputStream out = socket.getOutputStream();
				out.write("上传失败".getBytes());
			} catch (IOException e1) {
			}
		} finally {
			try {
				// 关闭socket对象
				if(fos != null)
					fos.close();
				if(socket != null)
					socket.close();
				
			} catch (IOException e) {
			}
		}
	}
}

```

## 14. 小结

- UDP
  - DatagramSocket
  - DatagramPacket
- TCP
  - ServerSocket
  - Socket


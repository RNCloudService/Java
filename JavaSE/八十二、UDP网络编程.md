# 八十二、UDP网络编程

**说明**

- DatagramSocket 和 DatagramPacket 两个类实现了基于UDP协议的网络程序。
- UDP 数据报通过数据报套接字 DatagramSocket 发送和接收，系统不保证UDP数据报一定能够安
- 全送到目的地，也不确定什么时候可以抵达。
- DatagramPacket 对象封装了UDP数据报，在数据报中包含了发送端的IP地址和端口号以及接收端
- 的IP地址和端口号。
- UDP协议中每个数据报都给出了完整的地址信息，因此无需建立发送方和接收方的连接。如同发快
- 递包裹一样。

**案例一**

发送方

```java
import java.net.DatagramPacket; 

import java.net.DatagramSocket; 

import java.net.InetAddress; 

public class UDPSenderDemo01 { 

public static void main(String[] args) throws Exception { 

//1. 建立DatagramSocket 

DatagramSocket socket = new DatagramSocket(); 

//2. 封装数据包 

String msg = "UDPSender==>"; 

byte[] data = msg.getBytes(); 

InetAddress inet = InetAddress.getByName("127.0.0.1"); 

int port = 9090; 

DatagramPacket packet = new 

DatagramPacket(data,0,data.length,inet,port); 

//3. 通过 Socket 发送 packet 

socket.send(packet); 

//4. 关闭socket 

socket.close(); 

} 

} 
```

接收方

```java
import java.net.DatagramPacket; 

import java.net.DatagramSocket; 

public class UDPReceiverDemo01 { 

public static void main(String[] args) throws Exception { 

//1. 建立DatagramSocket,开放端口 

DatagramSocket socket = new DatagramSocket(9090); 

//2. 接收数据 

byte[] buffer = new byte[1024]; 

DatagramPacket packet = new DatagramPacket(buffer,0,buffer.length); 

socket.receive(packet); 

//3. 输出数据 

// packet.getData() : 获取packet中的数据 

System.out.println(new String(packet.getData(), 0, 

packet.getLength())); 

//4. 关闭socket 

socket.close(); 

} 

} 
```

**案例二：在线咨询**

客户端

```java
import java.io.BufferedReader; 

import java.io.InputStreamReader; 

import java.net.DatagramPacket; 

import java.net.DatagramSocket; 

import java.net.InetSocketAddress; 

public class UdpTalkClient { 

public static void main(String[] args) throws Exception { 

System.out.println("发送方启动中...."); 

//1. 使用DatagramSocket 指定端口，创建发送端 

DatagramSocket socket = new DatagramSocket(8888); 

//2. 准备数据，转成字节数组 

BufferedReader reader = new BufferedReader(new 

InputStreamReader(System.in)); 

while (true){ 

String data = reader.readLine(); 

byte[] datas = data.getBytes(); 

//3. 封装成DatagramPacket包裹，需要指定目的地 

DatagramPacket packet = new DatagramPacket(datas,0,datas.length, 

new InetSocketAddress("localhost",6666)); 

//4. 发送包裹send 

socket.send(packet); 

//退出判断 

if (data.equals("bye")){ 

break; 

} 

}

//5. 释放资源 

socket.close(); 

} 

} 
```

服务端

```java
import java.net.DatagramPacket; 

import java.net.DatagramSocket; 

public class UdpTalkServer { 

public static void main(String[] args) throws Exception { 

DatagramSocket socket = new DatagramSocket(6666); 

while (true) { 

try {

//准备接收包裹； 

byte[] container = new byte[1024]; 

DatagramPacket packet = new DatagramPacket(container, 0, 

container.length);

socket.receive(packet); //阻塞式接收包裹 

byte[] datas = packet.getData(); 

int len = packet.getLength(); 

String data = new String(datas,0,len); 

System.out.println(data); 

//退出判断 

if (data.equals("bye")){ 

break; 

} 

}catch (Exception e){ 

e.printStackTrace(); 

} 

}

socket.close(); 

} 

}
```

**需要两遍需要接受和发送，可以使用多线程来解决**

发送端多线程

```java
import java.io.BufferedReader; 

import java.io.InputStreamReader; 

import java.net.DatagramPacket; 

import java.net.DatagramSocket; 

import java.net.InetSocketAddress; 

import java.net.SocketException; 

//发送端 

public class TalkSend implements Runnable { 

private DatagramSocket socket; 

private BufferedReader reader; 

private String toIP; 

private int toPort; 

public TalkSend(int port,String toIP,int toPort) { 

this.toIP = toIP; 

this.toPort = toPort; 

try {

socket = new DatagramSocket(port); 

reader = new BufferedReader(new InputStreamReader(System.in)); 

} catch (SocketException e) { 

e.printStackTrace(); 

} 

}

@Override 

public void run() { 

while (true){ 

try {

String data = reader.readLine(); 

byte[] datas = data.getBytes(); 

//3. 封装成DatagramPacket包裹，需要指定目的地 

DatagramPacket packet = new 

DatagramPacket(datas,0,datas.length, 

new InetSocketAddress(this.toIP,this.toPort)); 

//4. 发送包裹send 

socket.send(packet); 

//退出判断 

if (data.equals("bye")){ 

break; 

} 

}catch (Exception e){ 

e.printStackTrace(); 

} 

}

//5. 释放资源 

socket.close(); 

} 

}
```

接收端多线程

```java
import java.net.DatagramPacket; 

import java.net.DatagramSocket; 

import java.net.SocketException; 

//接收端 

public class TalkReceive implements Runnable { 

private DatagramSocket socket; 

private String msgFrom; 

public TalkReceive(int port,String msgFrom) { 

this.msgFrom = msgFrom; 

try {

socket = new DatagramSocket(port); 

} catch (SocketException e) { 

e.printStackTrace(); 

} 

}

@Override 

public void run() { 

while (true) { 

try {

//准备接收包裹； 

byte[] container = new byte[1024]; 

DatagramPacket packet = new DatagramPacket(container, 0, 

container.length);

socket.receive(packet); //阻塞式接收包裹 

byte[] datas = packet.getData(); 

int len = packet.getLength(); 

String data = new String(datas,0,len); 

System.out.println(msgFrom+":"+data); 

//退出判断 

if (data.equals("bye")){ 

break; 

} 

}catch (Exception e){ 

e.printStackTrace(); 

} 

}

socket.close(); 

} 

} 
```

学生端

```java
public class TalkStudent { 

public static void main(String[] args) { 

new Thread(new TalkSend(7777,"localhost",9999)).start(); 

new Thread(new TalkReceive(8888,"老师")).start(); 

} 

} 
```

老师端

```java
public class TalkTeacher { 

public static void main(String[] args) { 

new Thread(new TalkReceive(9999,"学生")).start(); 

new Thread(new TalkSend(5555,"localhost",8888)).start(); 

} 

} 
```


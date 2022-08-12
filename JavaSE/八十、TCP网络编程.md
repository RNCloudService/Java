# 八十、TCP网络编程

**案例一**

需求：客户端发送信息给服务端，服务端将数据显示在控制台上。

```java
import java.io.IOException; 

import java.io.OutputStream; 

import java.net.InetAddress; 

import java.net.Socket; 

import java.net.UnknownHostException; 

//客户端 

public class TcpClientDemo01 { 

public static void main(String[] args) { 

Socket socket = null; 

OutputStream os = null; 

try {

//1. 连接服务器的地址 

InetAddress serverIP = InetAddress.getByName("127.0.0.1"); 

int port = 8899; 

//2. 创建一个Socket 

socket = new Socket(serverIP,port); 

//3. 创建一个输出流，向外写东西 

os = socket.getOutputStream(); 

os.write("你好,欢迎学习Java".getBytes()); 

} catch (UnknownHostException e) { 

e.printStackTrace(); 

} catch (IOException e) { 

e.printStackTrace(); 

} finally { 

//4. 关闭资源 

try {

if (os!=null){ 

os.close(); 

}

if (socket!=null){ 

socket.close(); 

} 

} catch (IOException e) { 

e.printStackTrace(); 

} 

} 

} 

}
```

服务端

```java
import javax.sound.midi.Soundbank; 

import java.io.ByteArrayOutputStream; 

import java.io.IOException; 

import java.io.InputStream; 

import java.net.ServerSocket; 

import java.net.Socket; 

import java.util.Base64; 

//服务端 

public class TcpServerDemo01 { 

public static void main(String[] args) { 

ServerSocket serverSocket = null; 

Socket accept = null; 

InputStream is = null; 

ByteArrayOutputStream baos = null; 

try {

//1. 开放服务器端口，创建ServerSocket 

serverSocket = new ServerSocket(8899); 

//2. 等待客户端的连接 

accept = serverSocket.accept(); 

//3. 读入客户端的消息, 

is = accept.getInputStream(); 

/*

回忆之前的IO流方案，弊端：存在中文，可能存在乱码。 

byte[] buffer = new byte[1024]; 

int len; 

while ((len=is.read(buffer))!=-1){ 

String str = new String(buffer,0,len); 

System.out.println(str); 

}

**/ 

baos = new ByteArrayOutputStream(); 

byte[] buffer = new byte[1024]; 

int len; 

while ((len=is.read(buffer))!=-1){ 

baos.write(buffer,0,len); 

}

System.out.println(baos.toString()); 

System.out.println( 

"数据来源地址："+accept.getInetAddress().getHostName()); 

} catch (IOException e) { 

e.printStackTrace(); 

} finally { 

//4. 关闭资源 

try {

if (baos!=null){ 

baos.close(); 

}

if (is!=null){ 

is.close(); 

}

if (accept!=null){ 

accept.close(); 

}

if (serverSocket!=null){ 

serverSocket.close(); 

} 

}catch (Exception e){ 

e.printStackTrace(); 

} 

} 

} 

} 
```

**案例二**

需求：客户端发送文件给服务器，服务端将文件保存在本地。

编写代码：客户端

```java
import java.io.File; 

import java.io.FileInputStream; 

import java.io.OutputStream; 

import java.net.InetAddress; 

import java.net.Socket; 

//客户端 

public class TcpClientDemo02 { 

public static void main(String[] args) throws Exception{ 

//1. 创建socket连接 

Socket socket = new Socket(InetAddress.getByName("127.0.0.1"), 

9090); 

//2. 创建一个输出流 

OutputStream os = socket.getOutputStream(); 

//3. 读取文件 

FileInputStream fis = new FileInputStream(new File("qinjiang.jpg")); 

//4. 写出文件 

byte[] buffer = new byte[1024]; 

int len; 

while ((len=fis.read(buffer))!=-1){ 

os.write(buffer,0,len); 

}

//5. 资源关闭,应该使用 try-catch-finally 

fis.close(); 

os.close(); 

socket.close(); 

} 

} 
```

服务端：

```java
import java.io.*; 

import java.net.ServerSocket; 

import java.net.Socket; 

//服务端 

public class TcpServerDemo02 { 

public static void main(String[] args) throws Exception { 

//1. 开启 ServerSocket 

ServerSocket serverSocket = new ServerSocket(9090); 

//2. 侦听 客户端 Socket 

Socket socket = serverSocket.accept(); 

//3. 获取输入流 

InputStream is = socket.getInputStream(); 

//4. 读取接收的文件并保存 

FileOutputStream fos = new FileOutputStream(new 

File("receive.jpg")); 

byte[] buffer = new byte[1024]; 

int len; 

while ((len=is.read(buffer))!=-1){ 

fos.write(buffer,0,len); 

}

//5.关闭资源,应该使用 try-catch-finally 

fos.close(); 

is.close(); 

socket.close(); 

serverSocket.close(); 

} 

}
```

**案例三**

需要在案例二的基础上，接收成功后，返回给客户端，接收成功！然后客户端才关闭连接！

客户端

```java
package com.kuang.lesson4; 

import java.io.*; 

import java.net.InetAddress; 

import java.net.Socket; 

//客户端 

public class TcpClientDemo03 { 

public static void main(String[] args) throws Exception{ 

//1. 创建socket连接 

Socket socket = new Socket(InetAddress.getByName("127.0.0.1"), 

9090); 

//2. 创建一个输出流 

OutputStream os = socket.getOutputStream(); 

//3. 读取文件 

FileInputStream fis = new FileInputStream(new File("qinjiang.jpg")); 

//4. 写出文件 

byte[] buffer = new byte[1024]; 

int len; 

while ((len=fis.read(buffer))!=-1){ 

os.write(buffer,0,len); 

}

//告诉服务器，我传输完了,关闭数据的输出，不然就会一直阻塞！ 

socket.shutdownOutput(); 

//先别着急关，等待服务器响应，响应到控制台，注意重复的变量问题！ 

InputStream inputStream = socket.getInputStream(); 

ByteArrayOutputStream baos = new ByteArrayOutputStream(); 

byte[] buffer2 = new byte[1024]; 

int len2; 

while ((len2=inputStream.read(buffer2))!=-1){ 

baos.write(buffer2,0,len2); 

}

System.out.println(baos.toString()); 

//5. 资源关闭,应该使用 try-catch-finally 

baos.close(); 

inputStream.close(); 

fis.close(); 

os.close(); 

socket.close(); 

} 

} 
```

服务端

```java
import java.io.File; 

import java.io.FileOutputStream; 

import java.io.InputStream; 

import java.io.OutputStream; 

import java.net.ServerSocket; 

import java.net.Socket; 

//服务端 

public class TcpServerDemo03 { 

public static void main(String[] args) throws Exception { 

//1. 开启 ServerSocket 

ServerSocket serverSocket = new ServerSocket(9090); 

//2. 侦听 客户端 Socket 

Socket socket = serverSocket.accept(); 

//3. 获取输入流 

InputStream is = socket.getInputStream(); 

//4. 读取接收的文件并保存 

FileOutputStream fos = new FileOutputStream(new 

File("receive2.jpg")); 

byte[] buffer = new byte[1024]; 

int len; 

while ((len=is.read(buffer))!=-1){ 

fos.write(buffer,0,len); 

}

//通知客户端接收成功 

OutputStream outputStream = socket.getOutputStream(); 

outputStream.write("文件已经成功收到，OK".getBytes()); 

//5.关闭资源,应该使用 try-catch-finally 

outputStream.close(); 

fos.close(); 

is.close(); 

socket.close(); 

serverSocket.close(); 

} 

}
```


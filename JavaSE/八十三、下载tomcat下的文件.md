# 八十三、下载tomcat下的文件

首先需要在tomcat中放入一个资源文件！

```java
import java.io.FileOutputStream; 

import java.io.InputStream; 

import java.net.HttpURLConnection; 

import java.net.URL; 

public class URLDemo02 { 

public static void main(String[] args) { 

try {

//1. 定位到服务器端的资源 

URL url = new 

URL("http://localhost:8080/helloworld/qinjiang.jpg"); 

//2. 创建连接 

HttpURLConnection connection = (HttpURLConnection) 

url.openConnection(); 

//3. 获取输入流 

InputStream is = connection.getInputStream(); 

//4. 写出文件 

FileOutputStream fos = new FileOutputStream("qinjiang2.jpg"); 

byte[] buffer = new byte[1024]; 

int len; 

while ((len=is.read(buffer))!=-1){ 

fos.write(buffer,0,len); 

}

//关闭资源 

fos.close(); 

is.close(); 

connection.disconnect(); //断开连接 

} catch (Exception e) { 

e.printStackTrace(); 

} 

} 

} 
```


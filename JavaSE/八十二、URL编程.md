# 八十二、URL编程

### url类

- URL （Uniform Resource Locator）： 统一资源定位符，它表示 internet 上某一资源的地址。
- 它是一种具体的URI，即URL可以用来标识一个资源，而且还指明了如何locate：定位这个资源。
- 通过URL 我们可以访问Internet上的各种网络资源，比如最常见的 www,ftp站点。浏览器通过解析
- 给定的URL可以在网络上查找相应的文件或其他资源。
- URL 的 基本结构由 5部分组成：

```
传输协议：//主机名：端口号/文件名 #片段名？参数列表 
```

片段名，即锚链接，比如我们去一些小说网站，可以直接定位到某个章节位置

参数列表格式 ： 参数名=参数值 & 参数名=参数值

```java
import java.net.MalformedURLException; 

import java.net.URL; 

public class URLDemo01 { 

public static void main(String[] args) { 

try {

URL url = new URL("http://localhost:8080/helloworld/index.jsp? 

username=kuangshen&password=123"); 

System.out.println(url.getProtocol()); //获取URL的协议名 

System.out.println(url.getHost()); //获取URL的主机名 

System.out.println(url.getPort()); //获取URL的端口号 

System.out.println(url.getPath()); //获取URL的文件路径 

System.out.println(url.getFile()); //获取URL的文件名 

System.out.println(url.getQuery()); //获取URL的查询名 

} catch (MalformedURLException e) { 

e.printStackTrace(); 

} 

} 

} 
```


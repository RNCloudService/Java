# 七十七、Inet Adderss

### ip地址：Inet Adderss

- 唯一的标识 internet 上的计算机 ( 通信实体 )
- 本地回环地址（hostAddress）：127.0.0.1 主机名 ( hostName )：localhost
- IP地址分类方式一 ： IPV4 IPV6
- IPV4：4个字节组成，4个0~255。大概42亿个， 30亿都在北美，亚洲4亿。2011年初已经用
- 尽。以点分十进制表示，如192.168.0.1
- IPV6：128位（16个字节），写成8个无符号整数，每个整数用四个十六进制位表示，数之间
- 用冒号 隔开，如：2001:0db8:3c4d:0015:0000:0000:1a2f:1a2b
- IP地址分类方式2：公网地址（万维网使用） 和 私有地址（局域网使用）。
- 192.168.开头的就是私有地址，范围即为 192.168.0.0 ~ 192.168.255.255，专门为组织机构
- 内部使用
- 【查看 JDK 帮助文档=> InetAddress类，代表IP】
- 特点：不便于记忆，我们常使用域名来访问：www.google.com

**代码：**InetAddressTest

```java
package com.kuang.lesson1; 

import java.net.InetAddress; 

import java.net.UnknownHostException; 

//IP 这个东西，怎么用Java对象表示 

public class InetAddressTest { 

public static void main(String[] args) { 

try {

//获得IP地址 

InetAddress inetAddresses1 = 

InetAddress.getByName("192.168.8.123"); 

System.out.println(inetAddresses1); 

InetAddress inetAddresses2 = 

InetAddress.getByName("www.baidu.com"); 

System.out.println(inetAddresses2); 

//获取本地IP 

InetAddress inetAddresses3 = InetAddress.getByName("127.0.0.1"); 

System.out.println(inetAddresses3); 

InetAddress inetAddresses4 = InetAddress.getByName("localhost"); 

System.out.println(inetAddresses4); 

InetAddress inetAddresses5 = InetAddress.getLocalHost(); 

System.out.println(inetAddresses5); 

//getHostName 

System.out.println(inetAddresses2.getHostName()); 

//getHostAddress 

System.out.println(inetAddresses2.getHostAddress()); 

//Canonical : 规范的 

System.out.println(inetAddresses2.getCanonicalHostName()); 

} catch (UnknownHostException e) { 

e.printStackTrace(); 

} 

} 

} 
```


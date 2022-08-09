# 六十五、Writer流

继承自Writer的流都是用于程序中输出数据，且数据的单位为字符（16bit）；

### 7.1.Writer的基本方法

```java
//向输出流中写入一个字节数据，该字节数据为参数b的低16位 

void write(int b) throws IOException 

//将一个字节类型的数组中的数据写入输出流 

void write(byte[] b) throws IOException 

//将一个字节类型的数组中的从指定位置（off）开始的len个字节写入到输出流 

void write(byte[] b,int off,int len) throws IOException 

//关闭流释放内存资源 

void close() throws IOException 

//将输出流中缓冲的数据全部写出到目的地 

void flush() throws IOException
```

### 7.2 演示

【演示：使用FileWriter（字符流）向指定文件中写入数据】 

```java
package com.kuang.chapter; 

/*使用FileWriter（字符流）向指定文件中写入数据写入数据时以1个字符为单位进行写入*/ 

import java.io.*; 

public class TestFileWriter{ 

public static void main(String args[]){ 

/*使用FileWriter输出流从程序把数据写入到Uicode.dat文件中 

使用FileWriter流向文件写入数据时是一个字符一个字符写入的*/ 

FileWriter fw = null; 

try{

fw = new FileWriter("E:\\教学\\班级 

\\Test\\Lesson2\\src\\com\\kuang\\chapter\\StudentNew.java"); 

//字符的本质是一个无符号的16位整数 

//字符在计算机内部占用2个字节 

//这里使用for循环把0～60000里面的所有整数都输出 

//这里相当于是把全世界各个国家的文字都0～60000内的整数的形式来表示 

for(int c=0;c<=60000;c++){ 

fw.write(c); 

//使用write(int c)把0～60000内的整数写入到指定文件内 

//调用write()方法时，我认为在执行的过程中应该使用了“(char)c”进行强制 

转换，即把整数转换成字符来显示 

//因为打开写入数据的文件可以看到，里面显示的数据并不是0～60000内的整 

数，而是不同国家的文字的表示方式 

}

/*使用FileReader(字符流)读取指定文件里面的内容 

读取内容时是以一个字符为单位进行读取的*/ 

int b = 0; 

long num = 0; 

FileReader fr = null; 

fr = new FileReader("E:\\教学\\班级 

\\Test\\Lesson2\\src\\com\\kuang\\chapter\\StudentNew.java"); 

while((b = fr.read())!= -1){ 

System.out.print((char)b + "\t"); 

num++; 

}

System.out.println(); 

System.out.println("总共读取了"+num+"个字符"); 

}catch(Exception e){ 

e.printStackTrace(); 

} 

} 

} 
```

FileReader和FileWriter这两个流都是字符流，都是以一个字符为单位进行输入和输出的。所以读取和写入占用2个字节的字符时都可以正常地显示出来，以上是以File(文件)这个类型为例对节点流进行了讲

解，所谓的节点流指定就是直接把输入流或输出插入到数据源上，直接往数据源里面写入数据或读取数据。
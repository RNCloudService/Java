# 六十二、InputStream(输入流)

具体的某一些管道，凡是以InputStream结尾的管道，都是以字节的形式向我们的程序输入

数据。

继承自InputStream的流都是用于向程序中输入数据，且数据的单位为字节（8bit）；下图中深色为节点流，浅色为处理流。

### 4.1.InputStream的基本方法

```java
//读取一个字节并以整数的形式返回（0~255） 

//如果返回-1就说明已经到了输入流的末尾 

int read() throws IOException 

//读取一系列字节并存储到一个数组buffer 

//返回实际读取的字节数，如果读取前已到输入流的末尾，则返回-1 

int read(byte[] buffer) throws IOException 

//读取length个字节 

//并存储到一个字节数组buffer，从length位置开始 

//返回实际读取的字节数，如果读取前以到输入流的末尾返回-1. 

int read(byte[] buffer,int offset,int length) throws IOException 

//关闭流释放内存资源 

void close() throws IOException 

//跳过n个字节不读，返回实际跳过的字节数 

long skip(long n) throws IOException 
```

 read()方法是一个字节一个字节地往外读，每读取一个字节，就处理一个字节。read(byte[] buffffer)方法读取数据时，先把读取到的数据填满这个byte[]类型的数组buffffer(buffffer是内存里面的一块缓冲区)，然后再处理数组里面的数据。这就跟我们取水一样，先用一个桶去接，等桶接满水后再处理桶里面的水。

如果是每读取一个字节就处理一个字节

### **4.2** **案例** 

以File(文件)这个类型作为讲解节点流的典型代表

**使用FileInputStream流来读取FileInputStream.java文件的内容**

```java
package com.kuang.chapter; 

import java.io.*; 

public class TestFileInputStream { 

public static void main(String args[]) { 

int b = 0;// 使用变量b来装调用read()方法时返回的整数 

FileInputStream in = null; 

// 使用FileInputStream流来读取有中文的内容时，读出来的是乱码，因为使用 

InputStream流里面的read()方法读取内容时是一个字节一个字节地读取的，而一个汉字是占用两个 

字节的，所以读取出来的汉字无法正确显示。 

// FileReader in = null; 

// 使用FileReader流来读取内容时，中英文都可以正确显示，因为Reader流里面的 

read()方法是一个字符一个字符地读取的，这样每次读取出来的都是一个完整的汉字，这样就可以正确 

显示了。 

try {

in = new FileInputStream("E:\\教学\\班级 

\\Test\\Lesson2\\src\\com\\kuang\\chapter\\Student.java"); 

// in = new FileReader("E:\\教学\\班级 

\\Test\\Lesson2\\src\\com\\kuang\\chapter\\Student.java"); 

} catch (FileNotFoundException e) { 

System.out.println("系统找不到指定文件！"); 

System.exit(-1);// 系统非正常退出 

}

long num = 0;// 使用变量num来记录读取到的字符数 

// 调用read()方法时会抛异常，所以需要捕获异常 

try {

while ((b = in.read()) != -1) { 

// 调用int read() throws Exception方法时，返回的是一个int类型的整 

数 

// 循环结束的条件就是返回一个值-1，表示此时已经读取到文件的末尾了。 

// System.out.print(b+"\t");//如果没有使用“(char)b”进行转换，那 

么直接打印出来的b就是数字，而不是英文和中文了 

System.out.print((char) b); 

// “char(b)”把使用数字表示的汉字和英文字母转换成字符输入 

num++; 

}

in.close();// 关闭输入流 

System.out.println(); 

System.out.println("总共读取了" + num + "个字节的文件"); 

} catch (IOException e1) { 

System.out.println("文件读取错误！"); 

} 

} 

} 
```


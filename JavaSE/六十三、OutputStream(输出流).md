# 六十三、OutputStream(输出流)

继承自OutputStream的流是用于程序中输出数据，且数据的单位为字节（8bit）：下图中深色的为节点流，浅色为处理流。

### **5.1.OutputStream的基本方法**

```java
//向输出流中写入一个字节数据，该字节数据为参数b的低8位 

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

### **5.2** 案例

【使用FileOutputStream流往一个文件里面写入数据】

```java
package com.kuang.chapter; 

import java.io.*; 

public class TestFileOutputStream { 

public static void main(String args[]) { 

int b = 0; 

FileInputStream in = null; 

FileOutputStream out = null; 

try {

in = new FileInputStream("E:\\教学\\班级 

\\Test\\Lesson2\\src\\com\\kuang\\chapter\\Student.java"); 

out = new FileOutputStream("E:\\教学\\班级 

\\Test\\Lesson2\\src\\com\\kuang\\chapter\\StudentNew.java"); 

// 指明要写入数据的文件，如果指定的路径中不存在StudentNew.java这样的文 

件，则系统会自动创建一个 

while ((b = in.read()) != -1) { 

out.write(b); 

// 调用write(int c)方法把读取到的字符全部写入到指定文件中去 

} 

in.close(); 

out.close(); 

} catch (FileNotFoundException e) { 

System.out.println("文件读取失败"); 

System.exit(-1);// 非正常退出 

} catch (IOException e1) { 

System.out.println("文件复制失败！"); 

System.exit(-1); 

}

System.out 

.println("Student.StudentNew.java里面"); 

} 

} 
```

FileInputStream和FileOutputStream这两个流都是字节流，都是以一个字节为单位进行输入和输出

的。所以对于占用2个字节存储空间的字符来说读取出来时就会显示成乱码。
# File类

1. java.io.File类：文件和目录路径名的抽象表示形式。

File类的常见构造方法：

```java
public File(String pathname) 
```

以pathname为路径创建File对象，如果pathname是相对路径，则默认的当前路径在系统属性user.dir

中存储。

1. File的静态属性String separator存储了当前系统的路径分隔符。

2. 通过File对象可以访问文件的属性。

   

```java
public boolean canRead() 

public boolean exists() 

public boolean isFile() 

public long lastModified() 

public String getName() 

public boolean canWrite() 

public boolean isDirectory() 

public boolean isHidden() 

public long length() 

public String getPath() 
```

1. 通过File对象创建空文件或目录（在该对象所指的文件或目录不存在的情况下）。

```java
public boolean createNewFile()throws IOException 

public boolean delete() 

public boolean mkdir()， mkdirs() 
```

1. 常见构造器，方法

【演示】

```java
import java.io.File; 

import java.io.IOException; 

public class TestFile { 

/**

\* File文件类 1.代表文件 2.代表目录 

*/ 

public static void main(String[] args) { 

File f = new File("d:/src3/TestObject.java"); 

File f2 = new File("d:/src3"); 

File f3 = new File(f2, "TestFile.java"); 

File f4 = new File(f2, "TestFile666.java"); 

File f5 = new File("d:/src3/aa/bb/cc/dd"); 

//f5.mkdirs(); 

f5.delete(); 

try {

f4.createNewFile(); 

System.out.println("文件创建成功！"); 

} catch (IOException e) { 

e.printStackTrace(); 

}

if (f.isFile()) { 

System.out.println("是一个文件！"); 

}

if (f2.isDirectory()) { 

System.out.println("是一个目录！"); 

}

if (f3.isFile()) { 

System.out.println("是一个文件奥"); 

} 

} 

}
```


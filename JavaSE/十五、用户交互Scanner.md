# 用户交互Scanner

### 1、Scanner对象

java.util.Scanner 是 Java5 的新特征，我们可以通过 Scanner 类来获取用户的输入。

创建 Scanner 对象的基本语法：

```java
Scanner s = new Scanner(System.in); 
```

### 2、next & nextLine

```java
使用next方式接收一下输入的数据

public static void main(String[] args) { 

//创建一个扫描器对象，用于接收键盘数据 

Scanner scanner = new Scanner(System.in); 

//next方式接收字符串 

System.out.println("Next方式接收:"); 

//判断用户还有没有输入字符 

if (scanner.hasNext()){ 

String str = scanner.next(); 

System.out.println("输入内容："+str); 

}

//凡是属于IO流的类如果不关闭会一直占用资源.要养成好习惯用完就关掉.就好像你接水完了要关 

水龙头一样.很多下载软件或者视频软件如果你不彻底关,都会自己上传下载从而占用资源,会觉得 

卡,这一个道理. 

scanner.close(); 

} 
```

使用另一个方法来接收数据：nextLine()

```java
public static void main(String[] args) { 

Scanner scan = new Scanner(System.in); 

// 从键盘接收数据 

// nextLine方式接收字符串 

System.out.println("nextLine方式接收："); 

// 判断是否还有输入 

if (scan.hasNextLine()) { 

String str2 = scan.nextLine(); 

System.out.println("输入内容：" + str2); 

}

scan.close(); 

} 
```

**两者区别：**

next():

1、一定要读取到有效字符后才可以结束输入。

2、对输入有效字符之前遇到的空白，next() 方法会自动将其去掉。

3、只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符。

4、next() 不能得到带有空格的字符串。

nextLine()： 

1、以Enter为结束符,也就是说 nextLine()方法返回的是输入回车之前的所有字符。

2、可以获得空白。



### 3、其他方法

如果要输入 int 或 flfloat 类型的数据，在 Scanner 类中也有支持，但是在输入之前最好先使用

hasNextXxx() 方法进行验证，再使用 nextXxx() 来读取：

```java
public static void main(String[] args) { 

Scanner scan = new Scanner(System.in); 

// 从键盘接收数据 

int i = 0; 

float f = 0.0f; 

System.out.print("输入整数："); 

if (scan.hasNextInt()) { 

// 判断输入的是否是整数 

i = scan.nextInt(); 

// 接收整数 

System.out.println("整数数据：" + i); 

} else { 

// 输入错误的信息 

System.out.println("输入的不是整数！"); 

}

System.out.print("输入小数："); 

if (scan.hasNextFloat()) { 

// 判断输入的是否是小数 

f = scan.nextFloat(); 

// 接收小数 

System.out.println("小数数据：" + f); 

} else { 

// 输入错误的信息 

System.out.println("输入的不是小数！"); 

}

scan.close(); 

} 

public static void main(String[] args) { 

//扫描器接收键盘数据 

Scanner scan = new Scanner(System.in); 

double sum = 0; //和 

int m = 0; //输入了多少个数字 

//通过循环判断是否还有输入，并在里面对每一次进行求和和统计 

while (scan.hasNextDouble()) { 

double x = scan.nextDouble(); 

m = m + 1; 

sum = sum + x; 

}

System.out.println(m + "个数的和为" + sum); 

System.out.println(m + "个数的平均值是" + (sum / m)); 

scan.close(); 

} 
```


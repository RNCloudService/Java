# String类

### **1、String概述**

在API中是这样描述： String 类代表字符串。Java 程序中的所有字符串字面值（如 "abc" ）都作为此类的实例实现。 字符串是

常量；它们的值在创建之后不能更改。字符串缓冲区支持可变的字符串。因为 String 对象是不可变的，

所以可以共享。

```java
public final class String 

implements java.io.Serializable, Comparable<String>, CharSequence { 

} 
```

【String的成员变量】

```java
//String的属性值 

private final char value[]; 

//数组被使用的开始位置 

private final int offset; 

//String中元素的个数 

private final int count; 

//String类型的hash值 

private int hash; // Default to 0

private static final long serialVersionUID = -6849794470754667710L; 

private static final ObjectStreamField[] serialPersistentFields = 

new ObjectStreamField[0]; 
```

从源码看出String底层使用一个字符数组来维护的。

成员变量可以知道String类的值是fifinal类型的，不能被改变的，所以只要一个值改变就会生成一个新的

String类型对象，存储String数据也不一定从数组的第0个元素开始的，而是从offffset所指的元素开始

【String的构造方法】

```java
String() 

//初始化一个新创建的 String 对象，使其表示一个空字符序列。 

String(byte[] bytes) 

//通过使用平台的默认字符集解码指定的 byte 数组，构造一个新的 String。 

String(byte[] bytes, Charset charset) 

//通过使用指定的 charset 解码指定的 byte 数组，构造一个新的 String。 

String(byte[] bytes, int offset, int length) 

//通过使用平台的默认字符集解码指定的 byte 子数组，构造一个新的 String。 

String(byte[] bytes, int offset, int length, Charset charset) 

//通过使用指定的 charset 解码指定的 byte 子数组，构造一个新的 String。 

String(byte[] bytes, int offset, int length, String charsetName) 

//通过使用指定的字符集解码指定的 byte 子数组，构造一个新的 String。 

String(byte[] bytes, String charsetName) 

//通过使用指定的 charset 解码指定的 byte 数组，构造一个新的 String。 

String(char[] value) 

//分配一个新的 String，使其表示字符数组参数中当前包含的字符序列。 

String(char[] value, int offset, int count) 

//分配一个新的 String，它包含取自字符数组参数一个子数组的字符。 

String(int[] codePoints, int offset, int count) 

//分配一个新的 String，它包含 Unicode 代码点数组参数一个子数组的字符。 

String(String original) 

//初始化一个新创建的 String 对象，使其表示一个与参数相同的字符序列；换句话说，新创建 

的字符串是该参数字符串的副本。 

String(StringBuffer buffer) 

//分配一个新的字符串，它包含字符串缓冲区参数中当前包含的字符序列。 

String(StringBuilder builder) 

//分配一个新的字符串，它包含字符串生成器参数中当前包含的字符序列。
```

### 2、创建字符串对象方式

直接赋值方式创建对象是在方法区的常量池

```
String str="hello";//直接赋值的方式 
```

通过构造方法创建字符串对象是在堆内存

```java
String str=new String("hello");//实例化的方式 
```

【两种实例化方式的比较】

1. 编写代码比较

```java
public static void main(String[] args) { 

String str1 = "Lance"; 

String str2 = new String("Lance"); 

String str3 = str2; //引用传递，str3直接指向st2的堆内存地址 

String str4 = "Lance"; 

/** 

\* ==: 

\* 基本数据类型：比较的是基本数据类型的值是否相同 

\* 引用数据类型：比较的是引用数据类型的地址值是否相同 

\* 所以在这里的话：String类对象==比较，比较的是地址，而不是内容 

*/ 

System.out.println(str1==str2);//false 

System.out.println(str1==str3);//false 

System.out.println(str3==str2);//true 

System.out.println(str1==str4);//true 

} 
```

【字符串常量池】

在字符串中，如果采用直接赋值的方式（String str="Lance"）进行对象的实例化，则会将匿名对象

“Lance”放入对象池，每当下一次对不同的对象进行直接赋值的时候会直接利用池中原有的匿名对象，我

们可以用对象手工入池；

```java
public static void main(String args[]){ 

String str =new String("Lance").intern();//对匿名对象"hello"进行手工入池操作 

String str1="Lance"; 

System.out.println(str==str1);//true 

} 
```

【两种实例化方式的区别】

1. 直接赋值（String str = "hello"）：只开辟一块堆内存空间，并且会自动入池，不会产生垃圾。

2. 构造方法（String str= new String("hello");）:会开辟两块堆内存空间，其中一块堆内存会变成垃圾

被系统回收，而且不能够自动入池，需要通过public String intern();方法进行手工入池。

3. 在开发的过程中不会采用构造方法进行字符串的实例化。

【避免空指向】

首先了解： == 和public boolean equals()比较字符串的区别

==在对字符串比较的时候，对比的是内存地址，而equals比较的是字符串内容，在开发的过程中，

equals()通过接受参数，可以避免空指向。

```java
String str = null; 

if(str.equals("hello")){//此时会出现空指向异常 

... 

}

if("hello".equals(str)){//此时equals会处理null值，可以避免空指向异常 

... 

} 
```

【String类对象一旦声明则不可以改变；而改变的只是地址，原来的字符串还是存在的，并且产生垃

圾】

### **3、String常用的方法**

**1、String的判断**

【常用方法】

```java
boolean equals(Object obj)：比较字符串的内容是否相同 

boolean equalsIgnoreCase(String str)： 比较字符串的内容是否相同,忽略大小写 

boolean startsWith(String str)： 判断字符串对象是否以指定的str开头 

boolean endsWith(String str)： 判断字符串对象是否以指定的str结尾 
```

【演示】

```java
public static void main(String[] args) { 

// 创建字符串对象 

String s1 = "hello"; 

String s2 = "hello"; 

String s3 = "Hello"; 

// boolean equals(Object obj):比较字符串的内容是否相同 

System.out.println(s1.equals(s2)); //true 

System.out.println(s1.equals(s3)); //false 

System.out.println("-----------"); 

// boolean equalsIgnoreCase(String str):比较字符串的内容是否相同,忽略大小写 

System.out.println(s1.equalsIgnoreCase(s2)); //true 

System.out.println(s1.equalsIgnoreCase(s3)); //true 

System.out.println("-----------"); 

// boolean startsWith(String str):判断字符串对象是否以指定的str开头 

System.out.println(s1.startsWith("he")); //true 

System.out.println(s1.startsWith("ll")); //false 

} 
```

### **2、String的截取**

【常用方法】

```java
int length():获取字符串的长度，其实也就是字符个数 

char charAt(int index):获取指定索引处的字符 

int indexOf(String str):获取str在字符串对象中第一次出现的索引 

String substring(int start):从start开始截取字符串 

String substring(int start,int end):从start开始，到end结束截取字符串。包括start， 

不包括end 
```

【演示】

```java
public static void main(String args[]) { 

// 创建字符串对象 

String s = "helloworld"; 

// int length():获取字符串的长度，其实也就是字符个数 

System.out.println(s.length()); //10 

System.out.println("--------"); 

// char charAt(int index):获取指定索引处的字符 

System.out.println(s.charAt(0)); //h 

System.out.println(s.charAt(1)); //e 

System.out.println("--------"); 

// int indexOf(String str):获取str在字符串对象中第一次出现的索引 

System.out.println(s.indexOf("l")); //2 

System.out.println(s.indexOf("owo")); //4 

System.out.println(s.indexOf("ak")); //-1 

System.out.println("--------"); 

// String substring(int start):从start开始截取字符串 

System.out.println(s.substring(0)); //helloworld 

System.out.println(s.substring(5)); //world 

System.out.println("--------"); 

// String substring(int start,int end):从start开始，到end结束截取字符串 

System.out.println(s.substring(0, s.length())); //helloworld 

System.out.println(s.substring(3, 8)); //lowor 

} 
```

### **3、String的转换**

【常用方法】

```java
char[] toCharArray()：把字符串转换为字符数组 

String toLowerCase()：把字符串转换为小写字符串 

String toUpperCase()：把字符串转换为大写字符串
```

【演示】

```java
public static void main(String args[]) { 

// 创建字符串对象 

String s = "abcde"; 

// char[] toCharArray():把字符串转换为字符数组 

char[] chs = s.toCharArray(); 

for (int x = 0; x < chs.length; x++) { 

System.out.println(chs[x]); 

}

System.out.println("-----------"); 

// String toLowerCase():把字符串转换为小写字符串 

System.out.println("HelloWorld".toLowerCase()); 

// String toUpperCase():把字符串转换为大写字符串 

System.out.println("HelloWorld".toUpperCase()); 

} 
```

### **4、其他方法**

【常用方法】

```java
去除字符串两端空格：String trim() 

按照指定符号分割字符串：String[] split(String str) 
```

【演示】

```java
public static void main(String args[]) { 

// 创建字符串对象 

String s1 = "helloworld"; 

String s2 = " helloworld "; 

String s3 = " hello world "; 

System.out.println("---" + s1 + "---"); 

System.out.println("---" + s1.trim() + "---"); 

System.out.println("---" + s2 + "---"); 

System.out.println("---" + s2.trim() + "---"); 

System.out.println("---" + s3 + "---"); 

System.out.println("---" + s3.trim() + "---"); 

System.out.println("-------------------"); 

// String[] split(String str) 

// 创建字符串对象 

String s4 = "aa,bb,cc"; 

String[] strArray = s4.split(","); 

for (int x = 0; x < strArray.length; x++) { 

System.out.println(strArray[x]); 

} 

}
```

### **4、String的不可变性**

`Strings are constant; their values cannot be changed after they are created.` 

String是个常量，从一出生就注定不可变。

```java
public static void main(String[] args) { 

String a = "abc"; 

String b = "abc"; 

String c = new String("abc"); 

System.out.println(a==b); //true 

System.out.println(a.equals(b)); //true 

System.out.println(a==c); //false 

System.out.println(a.equals(c)); //true 

}
```

【分析】

因为String太过常用，JAVA类库的设计者在实现时做了个小小的变化，即采用了享元模式,每当生成一个

新内容的字符串时，他们都被添加到一个共享池中，当第二次再次生成同样内容的字符串实例时，就共

享此对象，而不是创建一个新对象，但是这样的做法仅仅适合于通过=符号进行的初始化。

需要说明一点的是，在object中，equals()是用来比较内存地址的，但是String重写了equals()方

法，用来比较内容的，即使是不同地址，只要内容一致，也会返回true，这也就是为什么a.equals(c)返 

回true的原因了。

【String不可变的好处】

可以实现多个变量引用堆内存中的同一个字符串实例，避免创建的开销。

我们的程序中大量使用了String字符串，有可能是出于安全性考虑。

大家都知道HashMap中key为String类型，如果可变将变的多么可怕。

当我们在传参的时候，使用不可变类不需要去考虑谁可能会修改其内部的值，如果使用可变类的

话，可能需要每次记得重新拷贝出里面的值，性能会有一定的损失。

### **5、字符串常量池**

【字符串常量池概述】

1. 常量池表（Constant_Pool table）

 Class文件中存储所有常量（包括字符串）的table。这是Class文件中的内容，还不是运行时的内容，不

要理解它是个池子，其实就是Class文件中的字节码指令。

1. 运行时常量池（Runtime Constant Pool） 

 JVM内存中方法区的一部分，这是运行时的内容。这部分内容（绝大部分）是随着JVM运行时候，从常

量池转化而来，每个Class对应一个运行时常量池。上一句中说绝大部分是因为：除了 Class中常量池内

容，还可能包括动态生成并加入这里的内容。

1. 字符串常量池（String Pool）

这部分也在方法区中，但与Runtime Constant Pool不是一个概念，String Pool是JVM实例全局共享

的，全局只有一个。JVM规范要求进入这里的String实例叫“被驻留的interned string”，各个JVM可以有

不同的实现，HotSpot是设置了一个哈希表StringTable来引用堆中的字符串实例，被引用就是被驻留。

【亨元模式】

其实字符串常量池这个问题涉及到一个设计模式，叫“享元模式”，顾名思义 - - - > 共享元素模式

也就是说：一个系统中如果有多处用到了相同的一个元素，那么我们应该只存储一份此元素，而让所有

地方都引用这一个元素

 Java

中String部分就是根据享元模式设计的，而那个存储元素的地方就叫做“字符串常量池 - String Pool”

【详细分析】

```java
int x = 10; 

String y = "hello"; 
```

1. 首先， 10 和 "hello" 会在经过javac（或者其他编译器）编译过后变为Class文件中

constant_pool table 的内容

2. 当我们的程序运行时，也就是说JVM运行时，每个Class constant_pool table 中的内容会被加

载到JVM内存中的方法区中各自Class的 Runtime Constant Pool。 

3. 一个没有被String Pool包含的Runtime Constant Pool中的字符串（这里是"hello"）会被加入到

String Pool中（HosSpot使用hashtable引用方式），步骤如下：

1. 在Java Heap（堆）中根据"hello"字面量create一个字符串对象

2. 将字面量"hello"与字符串对象的引用在hashtable中关联起来键 - 值

形式是："hello" = 对象的引用地址。

另外来说，当一个新的字符串出现在Runtime Constant Pool中时怎么判断需不需要在Java Heap中创建

新对象呢？

策略是这样：会先去根据equals来比较Runtime Constant Pool中的这个字符串是否和String Pool中某

一个是相等的（也就是找是否已经存在），如果有那么就不创建，直接使用其引用；反之，就如同上面

的第三步。如此，就实现了享元模式，提高的内存利用效率。

举例：

```java
使用String s = new String("hello");会创建几个对象 

答：会创建2个对象 

首先，出现了字面量"hello"，那么去String Pool中查找是否有相同字符串存在，因为程序就这一行 

代码所以肯定没有，那么就在Java Heap中用字面量"hello"首先创建1个String对象。 

接着，new String("hello")，关键字new又在Java Heap中创建了1个对象，然后调用接收String 

参数的构造器进行了初始化。最终s的引用是这个String对象. 
```


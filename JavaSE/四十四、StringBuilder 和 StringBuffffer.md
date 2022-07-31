# **StringBuilder** **和** StringBuffffer

**1、概述**

```java
public final class StringBuilder 

extends AbstractStringBuilder 

implements java.io.Serializable, CharSequence{ 

} 
```

StringBuilder 是一个可变的字符序列。它继承于AbstractStringBuilder，实现了CharSequence接口。

StringBuffffer 也是继承于AbstractStringBuilder的子类；但是，StringBuilder和StringBuffffer不同，前者是非线程安全的，后者是线程安全的。

【源码概览】

```java
package java.lang; 

public final class StringBuilder 

extends AbstractStringBuilder 

implements java.io.Serializable, CharSequence { 

static final long serialVersionUID = 4383685877147921099L; 

// 构造函数。默认的字符数组大小是16。 

public StringBuilder() { 

super(16); 

}

// 构造函数。指定StringBuilder的字符数组大小是capacity。 

public StringBuilder(int capacity) { 

super(capacity); 

}

// 构造函数。指定字符数组大小=str长度+15，且将str的值赋值到当前字符数组中。 

public StringBuilder(String str) { 

super(str.length() + 16); 

append(str); 

}

// 构造函数。指定字符数组大小=seq长度+15，且将seq的值赋值到当前字符数组中。 

public StringBuilder(CharSequence seq) { 

this(seq.length() + 16); 

append(seq); 

}

// 追加“对象obj对应的字符串”。String.valueOf(obj)实际上是调用obj.toString() 

public StringBuilder append(Object obj) { 

return append(String.valueOf(obj)); 

}

// 追加“str”。 

public StringBuilder append(String str) { 

super.append(str); 

return this; 

}

// 追加“sb的内容”。 

private StringBuilder append(StringBuilder sb) { 

if (sb == null) 

return append("null"); 

int len = sb.length(); 

int newcount = count + len; 

if (newcount > value.length) 

expandCapacity(newcount); 

sb.getChars(0, len, value, count); 

count = newcount; 

return this; 

}

// 追加“sb的内容”。 

public StringBuilder append(StringBuffer sb) { 

super.append(sb); 

return this; 

}

// 追加“s的内容”。 

public StringBuilder append(CharSequence s) { 

if (s == null) 

s = "null"; 

if (s instanceof String) 

return this.append((String)s); 

if (s instanceof StringBuffer) 

return this.append((StringBuffer)s); 

if (s instanceof StringBuilder) 

return this.append((StringBuilder)s); 

return this.append(s, 0, s.length()); 

}

// 追加“s从start(包括)到end(不包括)的内容”。 

public StringBuilder append(CharSequence s, int start, int end) { 

super.append(s, start, end); 

return this; 

}

// 追加“str字符数组对应的字符串” 

public StringBuilder append(char[] str) { 

super.append(str); 

return this; 

}

// 追加“str从offset开始的内容，内容长度是len”

public StringBuilder append(char[] str, int offset, int len) { 

super.append(str, offset, len); 

return this; 

}

// 追加“b对应的字符串” 

public StringBuilder append(boolean b) { 

super.append(b); 

return this; 

}

// 追加“c” 

public StringBuilder append(char c) { 

super.append(c); 

return this; 

}

// 追加“i” 

public StringBuilder append(int i) { 

super.append(i); 

return this; 

}

// 追加“lng” 

public StringBuilder append(long lng) { 

super.append(lng); 

return this; 

}

// 追加“f” 

public StringBuilder append(float f) { 

super.append(f); 

return this; 

}

// 追加“d” 

public StringBuilder append(double d) { 

super.append(d); 

return this; 

}

// 追加“codePoint” 

public StringBuilder appendCodePoint(int codePoint) { 

super.appendCodePoint(codePoint); 

return this; 

}

// 删除“从start(包括)到end的内容” 

public StringBuilder delete(int start, int end) { 

super.delete(start, end); 

return this; 

}

// 删除“位置index的内容” 

public StringBuilder deleteCharAt(int index) { 

super.deleteCharAt(index); 

return this;

// “用str替换StringBuilder中从start(包括)到end(不包括)的内容” 

public StringBuilder replace(int start, int end, String str) { 

super.replace(start, end, str); 

return this; 

}

// “在StringBuilder的位置index处插入‘str中从offset开始的内容’，插入内容长度是 

len”

public StringBuilder insert(int index, char[] str, int offset, 

int len) 

{ 

super.insert(index, str, offset, len); 

return this; 

}

// “在StringBuilder的位置offset处插入obj对应的字符串” 

public StringBuilder insert(int offset, Object obj) { 

return insert(offset, String.valueOf(obj)); 

}

// “在StringBuilder的位置offset处插入str” 

public StringBuilder insert(int offset, String str) { 

super.insert(offset, str); 

return this; 

}

// “在StringBuilder的位置offset处插入str” 

public StringBuilder insert(int offset, char[] str) { 

super.insert(offset, str); 

return this; 

}

// “在StringBuilder的位置dstOffset处插入s” 

public StringBuilder insert(int dstOffset, CharSequence s) { 

if (s == null) 

s = "null"; 

if (s instanceof String) 

return this.insert(dstOffset, (String)s); 

return this.insert(dstOffset, s, 0, s.length()); 

}

// “在StringBuilder的位置dstOffset处插入's中从start到end的内容'” 

public StringBuilder insert(int dstOffset, CharSequence s, 

int start, int end) 

{ 

super.insert(dstOffset, s, start, end); 

return this; 

}

// “在StringBuilder的位置Offset处插入b” 

public StringBuilder insert(int offset, boolean b) { 

super.insert(offset, b); 

return this; 

}

// “在StringBuilder的位置Offset处插入c” 

public StringBuilder insert(int offset, char c) {

super.insert(offset, c); 

return this; 

}

// “在StringBuilder的位置Offset处插入i” 

public StringBuilder insert(int offset, int i) { 

return insert(offset, String.valueOf(i)); 

}

// “在StringBuilder的位置Offset处插入l” 

public StringBuilder insert(int offset, long l) { 

return insert(offset, String.valueOf(l)); 

}

// “在StringBuilder的位置Offset处插入f” 

public StringBuilder insert(int offset, float f) { 

return insert(offset, String.valueOf(f)); 

}

// “在StringBuilder的位置Offset处插入d” 

public StringBuilder insert(int offset, double d) { 

return insert(offset, String.valueOf(d)); 

}

// 返回“str”在StringBuilder的位置 

public int indexOf(String str) { 

return indexOf(str, 0); 

}

// 从fromIndex开始查找，返回“str”在StringBuilder的位置 

public int indexOf(String str, int fromIndex) { 

return String.indexOf(value, 0, count, 

str.toCharArray(), 0, str.length(), 

fromIndex); 

}

// 从后向前查找，返回“str”在StringBuilder的位置 

public int lastIndexOf(String str) { 

return lastIndexOf(str, count); 

}

// 从fromIndex开始，从后向前查找，返回“str”在StringBuilder的位置 

public int lastIndexOf(String str, int fromIndex) { 

return String.lastIndexOf(value, 0, count, 

str.toCharArray(), 0, str.length(), 

fromIndex); 

}

// 反转StringBuilder 

public StringBuilder reverse() { 

super.reverse(); 

return this; 

}

public String toString() { 

// Create a copy, don't share the array 

return new String(value, 0, count); 

} 

// 序列化对应的写入函数 

private void writeObject(java.io.ObjectOutputStream s) 

throws java.io.IOException { 

s.defaultWriteObject(); 

s.writeInt(count); 

s.writeObject(value); 

}

// 序列化对应的读取函数 

private void readObject(java.io.ObjectInputStream s) 

throws java.io.IOException, ClassNotFoundException { 

s.defaultReadObject(); 

count = s.readInt(); 

value = (char[]) s.readObject(); 

} 

} 
```

### **2、常用方法**

### **1、insert**

```java
private static void testInsertAPIs(){ 

System.out.println("---------- testInsertAPIs -----------"); 

StringBuilder sbuilder = new StringBuilder(); 

// 在位置0处插入字符数组 

sbuilder.insert(0, new char[]{'a', 'b', 'c', 'd', 'e'}); 

// 在位置0处插入字符数组。0表示字符数组起始位置，3表示长度 

sbuilder.insert(0, new char[]{'A', 'B', 'C', 'D', 'E'}, 0, 3); 

// 在位置0处插入float 

sbuilder.insert(0, 1.414f); 

// 在位置0处插入double 

sbuilder.insert(0, 3.14159d); 

// 在位置0处插入boolean 

sbuilder.insert(0, true); 

// 在位置0处插入char 

sbuilder.insert(0, '\n'); 

// 在位置0处插入int 

sbuilder.insert(0, 100); 

// 在位置0处插入long 

sbuilder.insert(0, 12345L); 

// 在位置0处插入StringBuilder对象 

sbuilder.insert(0, new StringBuilder("StringBuilder")); 

// 在位置0处插入StringBuilder对象。6表示被在位置0处插入对象的起始位置(包括)，13是 

结束位置(不包括) 

sbuilder.insert(0, new StringBuilder("STRINGBUILDER"), 6, 13); 

// 在位置0处插入StringBuffer对象。 

sbuilder.insert(0, new StringBuffer("StringBuffer")); 

// 在位置0处插入StringBuffer对象。6表示被在位置0处插入对象的起始位置(包括)，12是结 

束位置(不包括) 

sbuilder.insert(0, new StringBuffer("STRINGBUFFER"), 6, 12); 

// 在位置0处插入String对象。 

sbuilder.insert(0, "String"); 

// 在位置0处插入String对象。1表示被在位置0处插入对象的起始位置(包括)，6是结束位置(不 

包括)

sbuilder.insert(0, "0123456789", 1, 6); 

sbuilder.insert(0, '\n'); 

// 在位置0处插入Object对象。此处以HashMap为例 

HashMap map = new HashMap(); 

map.put("1", "one"); 

map.put("2", "two"); 

map.put("3", "three"); 

sbuilder.insert(0, map); 

System.out.printf("%s\n\n", sbuilder); 

} 
```

### 2、append

```java
/**

\* StringBuilder 的append()示例 

*/ 

private static void testAppendAPIs() { 

System.out.println("------------------- testAppendAPIs ----------------- 

--");

StringBuilder sbuilder = new StringBuilder(); 

// 追加字符数组 

sbuilder.append(new char[]{'a','b','c','d','e'}); 

// 追加字符数组。0表示字符数组起始位置，3表示长度 

sbuilder.append(new char[]{'A','B','C','D','E'}, 0, 3); 

// 追加float 

sbuilder.append(1.414f); 

// 追加double 

sbuilder.append(3.14159d); 

// 追加boolean 

sbuilder.append(true); 

// 追加char 

sbuilder.append('\n'); 

// 追加int 

sbuilder.append(100); 

// 追加long 

sbuilder.append(12345L); 

// 追加StringBuilder对象 

sbuilder.append(new StringBuilder("StringBuilder")); 

// 追加StringBuilder对象。6表示被追加对象的起始位置(包括)，13是结束位置(不包括) 

sbuilder.append(new StringBuilder("STRINGBUILDER"), 6, 13); 

// 追加StringBuffer对象。 

sbuilder.append(new StringBuffer("StringBuffer")); 

// 追加StringBuffer对象。6表示被追加对象的起始位置(包括)，12是结束位置(不包括) 

sbuilder.append(new StringBuffer("STRINGBUFFER"), 6, 12); 

// 追加String对象。 

sbuilder.append("String"); 

// 追加String对象。1表示被追加对象的起始位置(包括)，6是结束位置(不包括) 

sbuilder.append("0123456789", 1, 6); 

sbuilder.append('\n'); 

// 追加Object对象。此处以HashMap为例 

HashMap map = new HashMap(); 

map.put("1", "one"); 

map.put("2", "two"); 

map.put("3", "three"); 

sbuilder.append(map); 

sbuilder.append('\n'); 

// 追加unicode编码 

sbuilder.appendCodePoint(0x5b57); // 0x5b57是“字”的unicode编码 

sbuilder.appendCodePoint(0x7b26); // 0x7b26是“符”的unicode编码 

sbuilder.appendCodePoint(0x7f16); // 0x7f16是“编”的unicode编码 

sbuilder.appendCodePoint(0x7801); // 0x7801是“码”的unicode编码 

System.out.printf("%s\n\n", sbuilder); 

} 
```

### 3、replace

```java
/**

\* StringBuilder 的replace()示例 

*/ 

private static void testReplaceAPIs() { 

System.out.println("------------------- testReplaceAPIs----------------- 

--");

StringBuilder sbuilder; 

sbuilder = new StringBuilder("0123456789"); 

sbuilder.replace(0, 3, "ABCDE"); 

System.out.printf("sbuilder=%s\n", sbuilder); 

sbuilder = new StringBuilder("0123456789"); 

sbuilder.reverse(); 

System.out.printf("sbuilder=%s\n", sbuilder); 

sbuilder = new StringBuilder("0123456789"); 

sbuilder.setCharAt(0, 'M'); 

System.out.printf("sbuilder=%s\n", sbuilder); 

System.out.println(); 

} 
```

### **4、delete**

```java
private static void testDeleteAPIs() { 

System.out.println("------------------- testDeleteAPIs ----------------- 

--");

StringBuilder sbuilder = new StringBuilder("0123456789");

// 删除位置0的字符，剩余字符是“123456789”。 

sbuilder.deleteCharAt(0); 

// 删除位置3(包括)到位置6(不包括)之间的字符，剩余字符是“123789”。 

sbuilder.delete(3,6); 

// 获取sb中从位置1开始的字符串 

String str1 = sbuilder.substring(1); 

// 获取sb中从位置3(包括)到位置5(不包括)之间的字符串 

String str2 = sbuilder.substring(3, 5); 

// 获取sb中从位置3(包括)到位置5(不包括)之间的字符串，获取的对象是CharSequence对 

象，此处转型为String 

String str3 = (String)sbuilder.subSequence(3, 5); 

System.out.printf("sbuilder=%s\nstr1=%s\nstr2=%s\nstr3=%s\n", 

sbuilder, str1, str2, str3); 

} 
```

### **5、index**

```java
/**

\* StringBuilder 中index相关API演示 

*/ 

private static void testIndexAPIs() { 

System.out.println("-------------------------------- testIndexAPIs ----- 

---------------------------"); 

StringBuilder sbuilder = new StringBuilder("abcAbcABCabCaBcAbCaBCabc"); 

System.out.printf("sbuilder=%s\n", sbuilder); 

// 1. 从前往后，找出"bc"第一次出现的位置 

System.out.printf("%-30s = %d\n", "sbuilder.indexOf(\"bc\")", 

sbuilder.indexOf("bc")); 

// 2. 从位置5开始，从前往后，找出"bc"第一次出现的位置 

System.out.printf("%-30s = %d\n", "sbuilder.indexOf(\"bc\", 5)", 

sbuilder.indexOf("bc", 5)); 

// 3. 从后往前，找出"bc"第一次出现的位置 

System.out.printf("%-30s = %d\n", "sbuilder.lastIndexOf(\"bc\")", 

sbuilder.lastIndexOf("bc")); 

// 4. 从位置4开始，从后往前，找出"bc"第一次出现的位置 

System.out.printf("%-30s = %d\n", "sbuilder.lastIndexOf(\"bc\", 4)", 

sbuilder.lastIndexOf("bc", 4)); 

System.out.println(); 

} 
```

### 6、其他API

```java
/**

\* StringBuilder 的其它API示例 

*/ 

private static void testOtherAPIs() { 

System.out.println("----------- testOtherAPIs -----------"); 

StringBuilder sbuilder = new StringBuilder("0123456789"); 

int cap = sbuilder.capacity(); 

System.out.printf("cap=%d\n", cap); 

/* 

capacity()返回的是字符串缓冲区的容量 

StringBuffer( ); 分配16个字符的缓冲区 

StringBuffer( int len ); 分配len个字符的缓冲区 

StringBuffer( String s ); 除了按照s的大小分配空间外,再分配16个 字符的缓冲区 

你的StringBuffer是用字符构造的，"0123456789"的长度是10另外再分配16个字符，所 

以一共是26。 

*/

char c = sbuilder.charAt(6); 

System.out.printf("c=%c\n", c); 

char[] carr = new char[4]; 

sbuilder.getChars(3, 7, carr, 0); 

for (int i=0; i<carr.length; i++){ 

System.out.printf("carr[%d]=%c ", i, carr[i]); 

}

System.out.println(); 

}
```

### **3、StringBuffffer**

和StringBulider用法差不多，

### 4、小结

【String、StringBuffffer、StringBuilder之间的区别】

首先需要说明的是：

String 字符串常量

StringBuffffer 字符串变量（线程安全）

StringBuilder 字符串变量（非线程安全）

在大多数情况下三者在执行速度方面的比较：StringBuilder > StringBuffffer > String

解释：

String 类型和 StringBuffffer 类型的主要性能区别其实在于 String 是不可变的对象, 因此在每次对 String类型进行改变的时候其实都等同于生成了一个新的 String 对象，然后将指针指向新的 String 对象，所以经常改变内容的字符串最好不要用 String ，因为每次生成对象都会对系统性能产生影响，特别当内存中

无引用对象多了以后， JVM 的 GC 就会开始工作，那速度是一定会相当慢的。

而如果是使用 StringBuffffer 类则结果就不一样了，每次结果都会对 StringBuffffer 对象本身进行操作，而不是生成新的对象，再改变对象引用。所以在一般情况下我们推荐使用 StringBuffffer ，特别是字符串对象经常改变的情况下。

在某些特别情况下， String 对象的字符串拼接其实是被 JVM 解释成了 StringBuffffer 对象的拼接，

所以这些时候 String 对象的速度并不会比 StringBuffffer 对象慢，而特别是以下的字符串对象生成中，String 效率是远要比 StringBuffffer 快的：

```java
String S1 = “This is only a” + “ simple” + “ test”; 

StringBuffer Sb = new StringBuilder(“This is only a”).append(“ 

simple”).append(“ test”);
```

其实就是：String S1 = “This is only a simple test”;

所以当然不需要太多的时间了。但大家这里要注意的是，如果你的字符串是来自另外的 String 对象

的话，速度就没那么快了，譬如：

String S2 = “This is only a”;

String S3 = “ simple”;

String S4 = “ test”;

大部分情况下StringBuilder的速度要大于StringBuffffer：

 java.lang.StringBuilder一个可变的字符序列是5.0新增的。（大多数情况下就是我们是在单线程下进行

的操作，所以大多数情况下是建议用StringBuilder而不用StringBuffffer的）此类提供一个与 StringBuffffer

兼容的 API，但不保证同步。该类被设计用作 StringBuffffer 的一个简易替换，用在字符串缓冲区被单个

线程使用的时候（这种情况很普遍）。如果可能，建议优先采用该类，因为在大多数实现中，它比

StringBuffffer 要快。两者的方法基本相同。

对于三者使用的总结：

1）如果要操作少量的数据用 = String

2）单线程操作字符串缓冲区 下操作大量数据 = StringBuilder

3）多线程操作字符串缓冲区 下操作大量数据 = StringBuffffer

**StringBuilder 与StringBuffffer的区别，StringBuilder与String的区别。**

1）StringBuilder效率高，线程不安全，StringBuffffer效率低，线程安全。

2）String是不可变字符串，StringBuilder是可变字符串。为什么有这样的差异，可以深入源码去解析，

比如String类内的 priver fifinal char value[] 等方法的原因。

3）如果是简单的声明一个字符串没有后续过多的操作，使用String,StringBuilder均可，若后续对字符穿

做频繁的添加，删除操作,或者是在循环当中动态的改变字符穿的长度应该用StringBuilder。使用String会产生多余的字符串，占用内存空间。
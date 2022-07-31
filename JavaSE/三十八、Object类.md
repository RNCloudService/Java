# Object类

由于所有的类都继承在Object类，因此省略了extends Object关键字。

该类中主要有以下方法:

toString()

getClass()

equals()

clone()

fifinalize(

其中toString(),getClass(),eq

### 1、clone()方法

clone顾名思义就是复制， 在Java语言中， clone方法被对象调用，所以会复制对象。所谓的复制对象，

首先要分配一个和源对象同样大小的空间，在这个空间中创建一个新的对象。那么在java语言中，有几

种方式可以创建对象呢？

使用new操作符创建一个对象

使用clone方法复制一个对象

那么这两种方式有什么相同和不同呢？ new操作符的本意是分配内存。程序执行到new操作符时， 首

先去看new操作符后面的类型，因为知道了类型，才能知道要分配多大的内存空间。分配完内存之后，

再调用构造函数，填充对象的各个域，这一步叫做对象的初始化，构造方法返回后，一个对象创建完

毕，可以把他的引用（地址）发布到外部，在外部就可以使用这个引用操纵这个对象。而clone在第一步是和new相似的， 都是分配内存，调用clone方法时，分配的内存和源对象（即调用clone方法的对象）相同，然后再使用原对象中对应的各个域，填充新对象的域， 填充完成之后，clone方法返回，一个新的相同的对象被创建，同样可以把这个新对象的引用发布到外部。

### clone与copy的区别

Employee tobby =new Employee(“CMTobby”,5000)

会有这样的赋值Employee cindyelf=tobby，这个时候只是简单了copy了一下reference，

cindyelf和tobby都指向内存中同一个object，这样cindyelf或者tobby的一个操作都可能影响到对方，如果我们通过cindyelf.raiseSalary()方法改变了salary域的值，那么tobby通过getSalary()方法

得到的就是修改之后的salary域的值，显然这不是我们愿意看到的。我们希望得到tobby的一个精确拷

贝，同时两者互不影响，这时候， 我们就可以使用Clone来满足我们的需求。Employee

cindy=tobby.clone()，这时会生成一个新的Employee对象，并且和tobby具有相同的属性值和方法。

### Shallow Clone与Deep Clone

主要是JAVA里除了8种基本类型传参数是值传递，其他的类对象传参数都是引用，我们有时候不希望在

方法里将参数改变，这是就需要在类中复写clone方法（实现深复制）。

 Clone是如何完成的呢？Object在对某个对象实施Clone时对其是一无所知的，它仅仅是简单地执行域

对域的copy，这就是Shallow Clone。这样，问题就来了咯。

以Employee为例，它里面有一个域hireDay不是基本数据类型的变量，而是一个reference变量，经过

Clone之后就会产生一个新的Date型的reference，

它和原始对象中对应的域指向同一个Date对象，这样克隆类就和原始类共享了一部分信息，而这样显然是不利的

这个时候我们就需要进行deep Clone了，对那些非基本类型的域进行特殊的处理，例如hireDay。可以重新定义Clone方法，对hireDay做特殊处理，如下代码所示：

```java
class Employee implements Cloneable { 

public Object clone() throws CloneNotSupportedException { 

Employee cloned = (Employee) super.clone(); 

cloned.hireDay = (Date) hireDay.clone() 

return cloned; 

} 

} 
```

### clone方法的保护机制

在Object中Clone()是被声明为protected的，这样做是有一定的道理的，以Employee类为例，通过声明为protected，就可以保证只有Employee类里面才能“克隆”Employee对象.

### clone方法的使用

什么时候使用shallow Clone，什么时候使用deep Clone，这个主要看具体对象的域是什么性质的，基

本型别还是reference variable

调用Clone()方法的对象所属的类(Class)必须implements Clonable接口，否则在调用Clone方法的时候会抛出CloneNotSupportedException

### 2、toString()方法

```java
public String toString() { 

return getClass().getName() + "@" + Integer.toHexString(hashCode()); 

} 
```

Object 类的 toString 方法返回一个字符串，该字符串由类名（对象是该类的一个实例）、at 标记符“@”

和此对象哈希码的无符号十六进制表示组成。

该方法用得比较多，**一般子类都有覆盖。**

```java
public static void main(String[] args){ 

Object o1 = new Object(); 

System.out.println(o1.toString()); 

} 
```

### 3、getClass()方法

```java
public final native Class<?> getClass(); 
```

返回次Object的运行时类类型。

不可重写，要调用的话，一般和getName()联合使用，如getClass().getName();

```java
public static void main(String[] args) { 

Object o = new Object(); 

System.out.println(o.getClass()); 

//class java.lang.Object 

} 
```

### 4、finalize()方法

```java
protected void finalize() throws Throwable { } 
```

该方法用于释放资源。因为无法确定该方法什么时候被调用，很少使用。

Java允许在类中定义一个名为fifinalize()的方法。它的工作原理是：一旦垃圾回收器准备好释放对象占用

的存储空间，将首先调用其fifinalize()方法。并且在下一次垃圾回收动作发生时，才会真正回收对象占用

的内存。

关于垃圾回收，有三点需要记住：

1、对象可能不被垃圾回收。只要程序没有濒临存储空间用完的那一刻，对象占用的空间就总也得不

到释放。

2、垃圾回收并不等于“析构”。

3、垃圾回收只与内存有关。使用垃圾回收的唯一原因是为了回收程序不再使用的内存。

### fifinalize()的用途：

无论对象是如何创建的，垃圾回收器都会负责释放对象占据的所有内存。

这就将对fifinalize()的需求限制到一种特殊情况，即通过某种创建对象方式以外的方式为对象分配了存储

空间。不过这种情况一般发生在使用“本地方法”的情况下，本地方法是一种在Java中调用非Java代码的方式

### 5、equals()方法

```java
public boolean equals(Object obj) { 

return (this == obj); 

} 
```

Object中的equals方法是直接判断this和obj本身的值是否相等，即用来判断调用equals的对象和形参

obj所引用的对象是否是同一对象，

所谓同一对象就是指内存中同一块存储单元，如果this和obj指向的hi同一块内存对象，则返回true,如果

this和obj指向的不是同一块内存，则返回false。

注意：即便是内容完全相等的两块不同的内存对象，也返回false。

如果是同一块内存，则object中的equals方法返回true,如果是不同的内存，则返回false

如果希望不同内存但相同内容的两个对象equals时返回true,则我们需要重写父类的equal方法

String类已经重写了object中的equals方法（这样就是比较内容是否相等了）

```java
public boolean equals(Object anObject) { 

if (this == anObject) { 

return true; 

}

if (anObject instanceof String) { 

String anotherString = (String)anObject; 

int n = value.length; 

if (n == anotherString.value.length) { 

char v1[] = value; 

char v2[] = anotherString.value; 

int i = 0; 

while (n-- != 0) { 

if (v1[i] != v2[i]) 

return false; 

i++; 

}

return true; 

} 

}

return false; 

} 
```

### **6、hashCode()方法**

返回该对象的哈希码值。

该方法用于哈希查找，可以减少在查找中使用equals的次数，重写了equals方法一般都要重写

hashCode方法。这个方法在一些具有哈希功能的Collection中用到。

一般必须满足obj1.equals(obj2)==true。可以推出obj1.hash Code() == obj2.hashCode()，但是

hashCode相等不一定就满足equals。不过为了提高效率，应该尽量使上面两个条件接近等价。

### 7 、wait()方法

```java
public final void wait() throws InterruptedException { 

wait(0); 

}

public final native void wait(long timeout) throws InterruptedException; 

public final void wait(long timeout, int nanos) throws InterruptedException 

{ 

if (timeout < 0) { 

throw new IllegalArgumentException("timeout value is negative"); 

}

if (nanos < 0 || nanos > 999999) { 

throw new IllegalArgumentException( 

"nanosecond timeout value out of range"); 

}

if (nanos > 0) { 

timeout++; 

}

wait(timeout); 

} 
```

wait方法就是使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。

wait()方法一直等待，直到获得锁或者被中断。wait(long timeout)设定一个超时间隔，

如果在规定时间内没有获得锁就返回。

调用该方法后当前线程进入睡眠状态，直到以下事件发生。



1）其他线程调用了该对象的notify方法。



2）其他线程调用了该对象的notifyAll方法。



3）其他线程调用了interrupt中断该线程。



4）时间间隔到了。

此时该线程就可以被调度了，如果是被中断的话就抛出一个InterruptedException异常。

### 8 、notify()方法

```java
public final native void notify(); 
```

该方法唤醒在该对象上等待的某个线程。

```java
public final native void notifyAll(); 
```

该方法唤醒在该对象上等待的所有线程。
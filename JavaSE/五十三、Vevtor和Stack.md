# 五十三、Vevtor和Stack

**1、锁机制：对象锁、方法锁、类锁**

对象锁就是方法锁：就是在一个类中的方法上加上synchronized关键字，这就是给这个方法加锁了。

类锁：锁的是整个类，当有多个线程来声明这个类的对象的时候将会被阻塞，直到拥有这个类锁的对象

被销毁或者主动释放了类锁。这个时候在被阻塞住的线程被挑选出一个占有该类锁，声明该类的对象。

其他线程继续被阻塞住。例如：在类A上有关键字synchronized，那么就是给类A加了类锁，线程1第一

个声明此类的实例，则线程1拿到了该类锁，线程2在想声明类A的对象，就会被阻塞。

2）在本文中，使用的是方法锁。

3）每个对象只有一把锁，有线程A，线程B，还有一个集合C类，线程A操作C拿到了集合中的锁(在

集合C中有用synchronized关键字修饰的)，并且还没有执行完，那么线程A就不会释放锁，当轮到线程B

去操作集合C中的方法时 ，发现锁被人拿走了，所以线程B只能等待那个拿到锁的线程使用完，然后才能

拿到锁进行相应的操作。

**1 Vector** 

**1、Vector概述**

通过API中可以知道：

1. Vector是一个可变化长度的数组

2. Vector增加长度通过的是capacity和capacityIncrement这两个变量，目前还不知道如何实现自动

扩增的，等会源码分析

3. Vector也可以获得iterator和listIterator这两个迭代器，并且他们发生的是fail-fast，而不是fail

safe，注意这里，不要觉得这个vector是线程安全就搞错了，具体分析在下面会说

4. Vector是一个线程安全的类，如果使用需要线程安全就使用Vector，如果不需要，就使用arrayList

5. Vector和ArrayList很类似，就少许的不一样，从它继承的类和实现的接口来看，跟arrayList一模一样。

注意：现在的版本已经是jdk1.7，还有更高的jdk1.8了，在开发中，建议不用vector，原因在文章的

结束会有解释，如果需要线程安全的集合类直接用java.util.concurrent包下的类。

**2、Vector源码分析**

【继承结构和层次关系】

```java
public class Vector<E> 

extends AbstractList<E> 

implements List<E>, RandomAccess, Cloneable, java.io.Serializable 

{

} 
```

【构造方法】

一共有四个构造方法。最后两个构造方法是collection Framwork的规范要写的构造方法。

构造方法作用：

1. 初始化存储元素的容器，也就是数组，elementData， 

2. 初始化capacityIncrement的大小，默认是0，这个的作用就是扩展数组的时候，增长的大小，为0

则每次扩展2倍 

【Vector()：空构造】

```java
/** 

\* Constructs an empty vector so that its internal data array 

\* has size {@code 10} and its standard capacity increment is 

\* zero. 

*/ 

//看注释，这个是一个空的Vector构造方法，所以让他使用内置的数组，这里还不知道什么是内置的数 

组，看它调用了自身另外一个带一个参数的构造器 

public Vector() { 

this(10); 

}
```

【Vector(int)】 

```java
/** 

\* Constructs an empty vector with the specified initial capacity and 

\* with its capacity increment equal to zero. 

*

\* @param initialCapacity the initial capacity of the vector 

\* @throws IllegalArgumentException if the specified initial capacity 

\* is negative 

*/ 

//注释说，给空的cector构造器用和带有一个特定初始化容量用的，并且又调用了另外一个带两个参数 

的构造器，并且给容量增长值(capacityIncrement=0)为0，查看vector中的变量可以发现 

capacityIncrement是一个成员变量 

public Vector(int initialCapacity) { 

this(initialCapacity, 0); 

} 
```

【ector(int，int)】 

```java
/** 

\* Constructs an empty vector with the specified initial capacity and 

\* capacity increment. 

*

\* @param initialCapacity the initial capacity of the vector 

\* @param capacityIncrement the amount by which the capacity is 

\* increased when the vector overflows 

\* @throws IllegalArgumentException if the specified initial capacity 

\* is negative 

*/ 

//构建一个有特定的初始化容量和容量增长值的空的Vector， 

public Vector(int initialCapacity, int capacityIncrement) { 

super();//调用父类的构造，是个空构造 

if (initialCapacity < 0)//小于0，会报非法参数异常：不合法的容量 

throw new IllegalArgumentException("Illegal Capacity: "+ 

initialCapacity); 

this.elementData = new Object[initialCapacity];//elementData是一个成员变量 

数组，初始化它，并给它初始化长度。默认就是10，除非自己给值。 

this.capacityIncrement = capacityIncrement;//capacityIncrement的意思是如果 

要扩增数组，每次增长该值，如果该值为0，那数组就变为两倍的原长度，这个之后会分析到 

} 
```

【Vector(Collection<? extends E> c)】

```java
/** 

\* Constructs a vector containing the elements of the specified 

\* collection, in the order they are returned by the collection's 

\* iterator. 

*

\* @param c the collection whose elements are to be placed into this 

\* vector 

\* @throws NullPointerException if the specified collection is null 

\* @since 1.2 

*/ 

//将集合c变为Vector，返回Vector的迭代器。 

public Vector(Collection<? extends E> c) { 

elementData = c.toArray(); 

elementCount = elementData.length; 

// c.toArray might (incorrectly) not return Object[] (see 6260652) 

if (elementData.getClass() != Object[].class) 

elementData = Arrays.copyOf(elementData, elementCount, 

Object[].class); 

} 
```

3、核心方法

【add()方法】

```java
/** 

\* Appends the specified element to the end of this Vector. 

*

\* @param e element to be appended to this Vector 

\* @return {@code true} (as specified by {@link Collection#add}) 

\* @since 1.2 

*/ 

//就是在vector中的末尾追加元素。但是看方法，synchronized，明白了为什么vector是线程安全 

的，因为在方法前面加了synchronized关键字，给该方法加锁了，哪个线程先调用它，其它线程就得 

等着，如果不清楚的就去看看多线程的知识，到后面我也会一一总结的。 

public synchronized boolean add(E e) { 

modCount++; 

//通过arrayList的源码分析经验，这个方法应该是在增加元素前，检查容量是否够用 

ensureCapacityHelper(elementCount + 1); 

elementData[elementCount++] = e; 

return true; 

} 
```

【ensureCapacityHelper(int)】 

```java
/** 

\* This implements the unsynchronized semantics of ensureCapacity. 

\* Synchronized methods in this class can internally call this 

\* method for ensuring capacity without incurring the cost of an 

\* extra synchronization. 

*

\* @see #ensureCapacity(int) 

*/ 

//这里注释解释，这个方法是异步(也就是能被多个线程同时访问)的，原因是为了让同步方法都能调用 

到这个检测容量的方法，比如add的同时，另一个线程调用了add的重载方法，那么两个都需要同时查询 

容量够不够，所以这个就不需要用synchronized修饰了。因为不会发生线程不安全的问题 

private void ensureCapacityHelper(int minCapacity) { 

// overflow-conscious code 

if (minCapacity - elementData.length > 0) 

//容量不够，就扩增，核心方法 

grow(minCapacity); 

} 
```

【grow(int)】

```java
//看一下这个方法，其实跟arrayList一样，唯一的不同就是在扩增数组的方式不一样，如果 

capacityIncrement不为0，那么增长的长度就是capacityIncrement，如果为0，那么扩增为2倍 

的原容量 

private void grow(int minCapacity) { 

// overflow-conscious code 

int oldCapacity = elementData.length; 

int newCapacity = oldCapacity + ((capacityIncrement > 0) ? 

capacityIncrement : oldCapacity); 

if (newCapacity - minCapacity < 0) 

newCapacity = minCapacity; 

if (newCapacity - MAX_ARRAY_SIZE > 0) 

newCapacity = hugeCapacity(minCapacity); 

elementData = Arrays.copyOf(elementData, newCapacity); 

} 
```

```java
public synchronized E get(int index) { 

if (index >= elementCount) 

throw new ArrayIndexOutOfBoundsException(index); 

return elementData(index); 

}
```

**2 Stack**

栈的意思。那么该类就是跟栈的用法一样了

```java
class Stack<E> extends Vector<E> {} 
```

查看api文档，很容易就能知道他的特性。就几个操作，出栈，入栈等，构造方法

也是空的，用的还是数组，父类中的构造，跟父类一样的扩增方式，并且它的方法也是同步的，所以也是线程安全

**3、总结Vector和Stack**

【Vector总结（通过源码分析）】

1. Vector线程安全是因为它的方法都加了synchronized关键字

2. Vector的本质是一个数组，特点能是能够自动扩增，扩增的方式跟capacityIncrement的值有关

3. 它也会fail-fast，还有一个fail-safe两个的区别在下面的list总结中会讲到。

【Stack的总结】

1. 对栈的一些操作，先进后出

2. 底层也是用数组实现的，因为继承了Vector

3. 也是线程安全的

**4 List总结**

【arrayList和LinkedList区别】

arrayList底层是用数组实现的顺序表，是随机存取类型，可自动扩增，并且在初始化时，数组的长

度是0，只有在增加元素时，长度才会增加。默认是10，不能无限扩增，有上限，在查询操作的时候性

能更好

LinkedList底层是用链表来实现的，是一个双向链表，注意这里不是双向循环链表,顺序存取类型。

在源码中，似乎没有元素个数的限制。应该能无限增加下去，直到内存满了在进行删除，增加操作时性

能更好。

两个都是线程不安全的，在iterator时，会发生fail-fast：快速失效。

【arrayList和Vector的区别】

arrayList线程不安全，在用iterator，会发生fail-fast

Vector线程安全，因为在方法前加了Synchronized关键字。也会发生fail-fast

【fail-fast和fail-safe区别和什么情况下会发生】

简单的来说：在java.util下的集合都是发生fail-fast，而在java.util.concurrent下的发生的都是fail

safe。

1）fail-fast

快速失败，例如在arrayList中使用迭代器遍历时，有另外的线程对arrayList的存储数组进行了改变，比

如add、delete、等使之发生了结构上的改变，所以Iterator就会快速报一个

java.util.ConcurrentModifificationException 异常（并发修改异常），这就是快速失败。

2）fail-safe

原理是在添加操作时会创建副本，在副本上进行添加操作，等迭代器遍历结束后，会将原引用

改为副本引用，所以我们在创建了一个list的迭代器，结果打印的就是123444了，

证明了确实改变成为了副本引用，后面为什么是三个4，原因是我们循环了3次，不久添加了3 

个4

**【为什么现在都不提倡使用vector了】**

1）vector实现线程安全的方法是在每个操作方法上加锁，这些锁并不是必须要的，在实际开发中，

一般都是通过锁一系列的操作来实现线程安全，也就是说将需要同步的资源放一起加锁来保证线程安

全。

2）如果多个Thread并发执行一个已经加锁的方法，但是在该方法中，又有vector的存在，vector

本身实现中已经加锁了，那么相当于锁上又加锁，会造成额外的开销。

3）就如上面第三个问题所说的，vector还有fail-fast的问题，也就是说它也无法保证遍历安全，在

遍历时又得额外加锁，又是额外的开销，还不如直接用arrayList，然后再加锁呢。

总结：Vector在你不需要进行线程安全的时候，也会给你加锁，也就导致了额外开销，所以在

jdk1.5之后就被弃用了，现在如果要用到线程安全的集合，都是从java.util.concurrent包下去拿相应的

类。
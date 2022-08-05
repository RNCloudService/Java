# 四十八、**ArrayList** **源码分析** 

**1、ArrayList概述**

1. ArrayList是可以动态增长和缩减的索引序列，它是基于数组实现的List类。

2. 该类封装了一个动态再分配的Object[]数组，每一个类对象都有一个capacity【容量】属性，表示

它们所封装的Object[]数组的长度，当向ArrayList中添加元素时，该属性值会自动增加。如果想

ArrayList中添加大量元素，可使用ensureCapacity方法一次性增加capacity，可以减少增加重分配

的次数提高性能。

3. ArrayList的用法和Vector向类似，但是Vector是一个较老的集合，具有很多缺点，不建议使用。

另外，ArrayList和Vector的区别是：ArrayList是线程不安全的，当多条线程访问同一个ArrayList集合

时，程序需要手动保证该集合的同步性，而Vector则是线程安全的。

**2、ArrayList的数据结构**

分析一个类的时候，数据结构往往是它的灵魂所在，理解底层的数据结构其实就理解了该类的实现思

路，具体的实现细节再具体分析。

**3、ArrayList源码分析**

1、继承结构和层次关系

IDEA快捷键：Ctrl+H

`

```java
public class ArrayList<E> extends AbstractList<E>` 

`implements List<E>, RandomAccess, Cloneable, java.io.Serializable{` 

`}
```

` 

 ArrayList **extends** AbstractList

 AbstractList **extends** AbstractCollection

所有类都继承Object 所以ArrayList的继承结构就是这样



【分析】

1. **为什么要先继承AbstractList，而让AbstractList先实现List？而不是让ArrayList直接实现List？**

这里是有一个思想，接口中全都是抽象的方法，而抽象类中可以有抽象方法，还可以有具体的实现方

法，正是利用了这一点，让AbstractList是实现接口中一些通用的方法，而具体的类，如ArrayList就继承

这个AbstractList类，拿到一些通用的方法，然后自己在实现一些自己特有的方法，这样一来，让代码更

简洁，就继承结构最底层的类中通用的方法都抽取出来，先一起实现了，减少重复代码。所以一般看到

一个类上面还有一个抽象类，应该就是这个作用。

1. ArrayList实现了哪些接口？

**RandomAccess接口：**这个是一个标记性接口，通过查看api文档，它的作用就是用来快速随机存取，

有关效率的问题，在实现了该接口的话，那么使用普通的for循环来遍历，性能更高，例如ArrayList。而

没有实现该接口的话，使用Iterator来迭代，这样性能更高，例如linkedList。所以这个标记性只是为了

让我们知道我们用什么样的方式去获取数据性能更

**Cloneable接口：**实现了该接口，就可以使用Object.Clone()方法了。

**Serializable接口：**实现该序列化接口，表明该类可以被序列化，什么是序列化？简单的说，就是能够

从类变成字节流传输，然后还能从字节流变成原来的类。

**2、类中的属性**

```java
public class ArrayList<E> extends AbstractList<E> 

implements List<E>, RandomAccess, Cloneable, java.io.Serializable 

{ 

// 版本号 

private static final long serialVersionUID = 8683452581122892189L; 

// 缺省容量 

private static final int DEFAULT_CAPACITY = 10; 

// 空对象数组 

private static final Object[] EMPTY_ELEMENTDATA = {}; 

// 缺省空对象数组 

private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {}; 

// 元素数组 

transient Object[] elementData; 

// 实际元素大小，默认为0 

private int size; 

// 最大数组容量 

private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8; 

} 
```

**3、构造方法**

通过IDEA查看源码，看到ArrayList有三个构造方法：

1. 无参构造方法

```java
/* 

Constructs an empty list with an initial capacity of ten. 

这里就说明了默认会给10的大小，所以说一开始arrayList的容量是10. 

*/

//ArrayList中储存数据的其实就是一个数组，这个数组就是elementData. 

public ArrayList() { 

super(); //调用父类中的无参构造方法，父类中的是个空的构造方法 

this.elementData = EMPTY_ELEMENTDATA; 

//EMPTY_ELEMENTDATA：是个空的Object[]， 将elementData初始化，elementData 

也是个Object[]类型。空的Object[]会给默认大小10，等会会解释什么时候赋值的。 

} 
```

1. 有参构造方法（1）

```java
/*

Constructs an empty list with the specified initial capacity. 

构造具有指定初始容量的空列表。 

@param initialCapacity the initial capacity of the list 

初始容量列表的初始容量 

@throws IllegalArgumentException if the specified initial capacity is 

negative

如果指定的初始容量为负，则为IllegalArgumentException 

*/

public ArrayList(int initialCapacity) { 

if (initialCapacity > 0) { 

////将自定义的容量大小当成初始化 initialCapacity 的大小 

this.elementData = new Object[initialCapacity]; 

} else if (initialCapacity == 0) { 

this.elementData = EMPTY_ELEMENTDATA; //等同于无参构造方法 

} else { 

////判断如果自定义大小的容量小于0，则报下面这个非法数据异常 

throw new IllegalArgumentException("Illegal Capacity: "+ 

initialCapacity); 

} 

} 
```

1. 有参构造方法 （2）

```java
/* 

Constructs a list containing the elements of the specified collection, 

in the order they are returned by the collection's iterator. 

按照集合迭代器返回元素的顺序构造包含指定集合的元素的列表。 

@param c the collection whose elements are to be placed into this list 

@throws NullPointerException if the specified collection is null 

*/

public ArrayList(Collection<? extends E> c) { 

elementData = c.toArray(); //转换为数组 

//每个集合的toarray()的实现方法不一样，所以需要判断一下，如果不是Object[].class类 

型，那么久需要使用ArrayList中的方法去改造一下。 

if ((size = elementData.length) != 0) { 

// c.toArray might (incorrectly) not return Object[] (see 6260652) 

if (elementData.getClass() != Object[].class) 

elementData = Arrays.copyOf(elementData, size, Object[].class); 

} else { 

// replace with empty array. 

this.elementData = EMPTY_ELEMENTDATA; 

} 

} 
```

Strudent exends Person ， ArrayList、 Person这里就是泛型 ， 我还有一个Collection、

由于这个Student继承了Person，那么根据这个构造方法，我就可以把这个Collection转换为ArrayList

这就是这个构造方法的作用 。

【总结】ArrayList的构造方法就做一件事情，就是初始化一下储存数据的容器，其实本质上就是一个数组，在其中就叫elementData。 






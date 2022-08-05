# 五十五、HashMap的源码分析

**1、HashMap的层次关系与继承结构**

【HashMap继承结构】

继承了一个abstractMap，也就是用来减轻实现Map接口的编写负担。

【实现接口】

```java
public class HashMap<K,V> extends AbstractMap<K,V> 

implements Map<K,V>, Cloneable, Serializable { 

} 
```

 Map<K,V>：在AbstractMap抽象类中已经实现过的接口，这里又实现，实际上是多余的。但每个集合

都有这样的错误，也没过大影响

 Cloneable：能够使用Clone()方法，在HashMap中，实现的是浅层次拷贝，即对拷贝对象的改变会影响

被拷贝的对象。

 Serializable：能够使之序列化，即可以将HashMap对象保存至本地，之后可以恢复状态。

**2、HashMap类的属性**

```java
public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, 

Cloneable, Serializable { 

// 序列号 

private static final long serialVersionUID = 362498820763181265L; 

// 默认的初始容量是16 

static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; 

// 最大容量 

static final int MAXIMUM_CAPACITY = 1 << 30; 

// 默认的填充因子 

static final float DEFAULT_LOAD_FACTOR = 0.75f; 

// 当桶(bucket)上的结点数大于这个值时会转成红黑树 

static final int TREEIFY_THRESHOLD = 8; 

// 当桶(bucket)上的结点数小于这个值时树转链表 

static final int UNTREEIFY_THRESHOLD = 6; 

// 桶中结构转化为红黑树对应的table的最小大小 

static final int MIN_TREEIFY_CAPACITY = 64; 

// 存储元素的数组，总是2的幂次倍 

transient Node<k,v>[] table; 

// 存放具体元素的集 

transient Set<map.entry<k,v>> entrySet; 

// 存放元素的个数，注意这个不等于数组的长度。 

transient int size; 

// 每次扩容和更改map结构的计数器 

transient int modCount; 

// 临界值 当实际大小(容量*填充因子)超过临界值时，会进行扩容 

int threshold; 

// 填充因子 

final float loadFactor; 

} 
```

**3、HashMap的构造方法**

有四个构造方法，构造方法的作用就是记录一下16这个数给threshold（这个数值最终会当作第一次数

组的长度。）和初始化加载因子。注意，hashMap中table数组一开始就已经是个没有长度的数组了。

构造方法中，并没有初始化数组的大小，数组在一开始就已经被创建了，构造方法只做两件事情，一个是初始化加载因子，另一个是用threshold记录下数组初始化的大小。

【HashMap()】 

```java
//看上面的注释就已经知道，DEFAULT_INITIAL_CAPACITY=16，DEFAULT_LOAD_FACTOR=0.75 

//初始化容量：也就是初始化数组的大小 

//加载因子：数组上的存放数据疏密程度。 

public HashMap() { 

this(DEFAULT_INITIAL_CAPACITY, DEFAULT_LOAD_FACTOR); 

} 
```

【HashMap(int)】 

```java
public HashMap(int initialCapacity) { 

this(initialCapacity, DEFAULT_LOAD_FACTOR); 

} 
```

【HashMap(int,flfloat)】 

```java
public HashMap(int initialCapacity, float loadFactor) { 

// 初始容量不能小于0，否则报错 

if (initialCapacity < 0) 

throw new IllegalArgumentException("Illegal initial capacity: " + 

initialCapacity); 

// 初始容量不能大于最大值，否则为最大值 

if (initialCapacity > MAXIMUM_CAPACITY) 

initialCapacity = MAXIMUM_CAPACITY; 

// 填充因子不能小于或等于0，不能为非数字 

if (loadFactor <= 0 || Float.isNaN(loadFactor)) 

throw new IllegalArgumentException("Illegal load factor: " + 

loadFactor); 

// 初始化填充因子 

this.loadFactor = loadFactor; 

// 初始化threshold大小 

this.threshold = tableSizeFor(initialCapacity); 

} 
```

【HashMap(Map<? extends K, ? extends V> m)】 

```java
public HashMap(Map<? extends K, ? extends V> m) { 

// 初始化填充因子 

this.loadFactor = DEFAULT_LOAD_FACTOR; 

// 将m中的所有元素添加至HashMap中 

putMapEntries(m, false); 

} 
```

【putMapEntries(Map<? extends K, ? extends V> m, boolean evict)函数将m的所有元素存入本

```java
HashMap实例中】

final void putMapEntries(Map<? extends K, ? extends V> m, boolean evict) { 

int s = m.size(); 

if (s > 0) { 

// 判断table是否已经初始化 

if (table == null) { // pre-size 

// 未初始化，s为m的实际元素个数 

float ft = ((float)s / loadFactor) + 1.0F; 

int t = ((ft < (float)MAXIMUM_CAPACITY) ? 

(int)ft : MAXIMUM_CAPACITY); 

// 计算得到的t大于阈值，则初始化阈值 

if (t > threshold) 

threshold = tableSizeFor(t); 

}

// 已初始化，并且m元素个数大于阈值，进行扩容处理 

else if (s > threshold) 

resize(); 

// 将m中的所有元素添加至HashMap中 

for (Map.Entry<? extends K, ? extends V> e : m.entrySet()) { 

K key = e.getKey(); 

V value = e.getValue(); 

putVal(hash(key), key, value, false, evict); 

} 

} 

} 
```

**4、常用方法**

【put(K key,V value)】 

```java
public V put(K key, V value) { 

return putVal(hash(key), key, value, false, true); 

} 
```

【putVal(int hash, K key, V value, boolean onlyIfAbsent,boolean evict)】 

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent, 

boolean evict) { 

Node<K,V>[] tab; Node<K,V> p; int n, i; 

// table未初始化或者长度为0，进行扩容 

if ((tab = table) == null || (n = tab.length) == 0) 

n = (tab = resize()).length; 

// (n - 1) & hash 确定元素存放在哪个桶中，桶为空，新生成结点放入桶中(此时，这个结点 

是放在数组中) 

if ((p = tab[i = (n - 1) & hash]) == null) 

tab[i] = newNode(hash, key, value, null); 

// 桶中已经存在元素 

else {

Node<K,V> e; K k; 

// 比较桶中第一个元素(数组中的结点)的hash值相等，key相等 

if (p.hash == hash && 

((k = p.key) == key || (key != null && key.equals(k)))) 

// 将第一个元素赋值给e，用e来记录 

e = p; 

// hash值不相等，即key不相等；为红黑树结点 

else if (p instanceof TreeNode) 

// 放入树中 

e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value); 

// 为链表结点 

else {

// 在链表最末插入结点 

for (int binCount = 0; ; ++binCount) { 

// 到达链表的尾部 

if ((e = p.next) == null) { 

// 在尾部插入新结点 

p.next = newNode(hash, key, value, null); 

// 结点数量达到阈值，转化为红黑树 

if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st 

treeifyBin(tab, hash); 

// 跳出循环 

break; 

}

// 判断链表中结点的key值与插入的元素的key值是否相等 

if (e.hash == hash && 

((k = e.key) == key || (key != null && key.equals(k)))) 

// 相等，跳出循环 

break; 

// 用于遍历桶中的链表，与前面的e = p.next组合，可以遍历链表 

p = e; 

} 

}

// 表示在桶中找到key值、hash值与插入元素相等的结点 

if (e != null) { 

// 记录e的value 

V oldValue = e.value; 

// onlyIfAbsent为false或者旧值为null 

if (!onlyIfAbsent || oldValue == null) 

//用新值替换旧值 

e.value = value; 

// 访问后回调 

afterNodeAccess(e); 

// 返回旧值 

return oldValue; 

} 

}

// 结构性修改 

++modCount; 

// 实际大小大于阈值则扩容 

if (++size > threshold) 

resize(); 

// 插入后回调 

afterNodeInsertion(evict); 

return null; 

} 
```

HashMap并没有直接提供putVal接口给用户调用，而是提供的put函数，而put函数就是通过putVal来插入元素的。

【get(Object key)】 

```java
public V get(Object key) { 

Node<K,V> e; 

return (e = getNode(hash(key), key)) == null ? null : e.value; 

} 
```

【getNode(int hash,Pbject key)】 

```java
final Node<K,V> getNode(int hash, Object key) { 

Node<K,V>[] tab; Node<K,V> first, e; int n; K k; 

// table已经初始化，长度大于0，根据hash寻找table中的项也不为空 

if ((tab = table) != null && (n = tab.length) > 0 && 

(first = tab[(n - 1) & hash]) != null) { 

// 桶中第一项(数组元素)相等 

if (first.hash == hash && // always check first node 

((k = first.key) == key || (key != null && key.equals(k)))) 

return first; 

// 桶中不止一个结点 

if ((e = first.next) != null) { 

// 为红黑树结点 

if (first instanceof TreeNode) 

// 在红黑树中查找 

return ((TreeNode<K,V>)first).getTreeNode(hash, key); 

// 否则，在链表中查找 

do {

if (e.hash == hash && 

((k = e.key) == key || (key != null && key.equals(k)))) 

return e; 

} while ((e = e.next) != null); 

} 

}

return null;

} 
```

 HashMap并没有直接提供getNode接口给用户调用，而是提供的get函数，而get函数就是通过

getNode来取得元素的

【resize方法】

```java
final Node<K,V>[] resize() { 

// 当前table保存 

Node<K,V>[] oldTab = table; 

// 保存table大小 

int oldCap = (oldTab == null) ? 0 : oldTab.length; 

// 保存当前阈值 

int oldThr = threshold; 

int newCap, newThr = 0; 

// 之前table大小大于0 

if (oldCap > 0) { 

// 之前table大于最大容量 

if (oldCap >= MAXIMUM_CAPACITY) { 

// 阈值为最大整形 

threshold = Integer.MAX_VALUE; 

return oldTab; 

}

// 容量翻倍，使用左移，效率更高 

else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY && 

oldCap >= DEFAULT_INITIAL_CAPACITY) 

// 阈值翻倍 

newThr = oldThr << 1; // double threshold 

}

// 之前阈值大于0 

else if (oldThr > 0) 

newCap = oldThr; 

// oldCap = 0并且oldThr = 0，使用缺省值（如使用HashMap()构造函数，之后再插入一个 

元素会调用resize函数，会进入这一步） 

else {

newCap = DEFAULT_INITIAL_CAPACITY; 

newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY); 

}

// 新阈值为0 

if (newThr == 0) { 

float ft = (float)newCap * loadFactor; 

newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY 

? 

(int)ft : Integer.MAX_VALUE); 

}

threshold = newThr; 

@SuppressWarnings({"rawtypes","unchecked"}) 

// 初始化table 

Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap]; 

table = newTab; 

// 之前的table已经初始化过 

if (oldTab != null) { 

// 复制元素，重新进行hash 

for (int j = 0; j < oldCap; ++j) { 

Node<K,V> e; 

if ((e = oldTab[j]) != null) { 

oldTab[j] = null; 

if (e.next == null) 

newTab[e.hash & (newCap - 1)] = e; 

else if (e instanceof TreeNode) 

((TreeNode<K,V>)e).split(this, newTab, j, oldCap); 

else { // preserve order 

Node<K,V> loHead = null, loTail = null; 

Node<K,V> hiHead = null, hiTail = null; 

Node<K,V> next; 

// 将同一桶中的元素根据(e.hash & oldCap)是否为0进行分割，分成两 

个不同的链表，完成rehash

do {

next = e.next; 

if ((e.hash & oldCap) == 0) { 

if (loTail == null) 

loHead = e; 

else

loTail.next = e; 

loTail = e; 

}

else {

if (hiTail == null) 

hiHead = e; 

else

hiTail.next = e; 

hiTail = e; 

} 

} while ((e = next) != null); 

if (loTail != null) { 

loTail.next = null; 

newTab[j] = loHead; 

}

if (hiTail != null) { 

hiTail.next = null; 

newTab[j + oldCap] = hiHead; 

} 

} 

} 

} 

}

return newTab; 

} 
```

进行扩容，会伴随着一次重新hash分配，并且会遍历hash表中所有的元素，是非常耗时的。在编写程

序中，要尽量避免resize。 

**4** **总结**

【关于数组扩容】

从putVal源代码中我们可以知道，当插入一个元素的时候size就加1，若size大于threshold的时候，就

会进行扩容。假设我们的capacity大小为32，loadFator为0.75,则threshold为24 = 32 * 0.75，

此时，插入了25个元素，并且插入的这25个元素都在同一个桶中，桶中的数据结构为红黑树，则还

有31个桶是空的，也会进行扩容处理，其实，此时，还有31个桶是空的，好像似乎不需要进行扩容处

理，但是是需要扩容处理的，因为此时我们的capacity大小可能不适当。我们前面知道，扩容处理会遍

历所有的元素，时间复杂度很高；前面我们还知道，经过一次扩容处理后，元素会更加均匀的分布在各

个桶中，会提升访问效率。所以，说尽量避免进行扩容处理，也就意味着，遍历元素所带来的坏处大于

元素在桶中均匀分布所带来的好处。

【总结】

1. 要知道hashMap在JDK1.8以前是一个链表散列这样一个数据结构，而在JDK1.8以后是一个数组加

链表加红黑树的数据结构。

2. 通过源码的学习，hashMap是一个能快速通过key获取到value值得一个集合，原因是内部使用的

是hash查找值得方法。
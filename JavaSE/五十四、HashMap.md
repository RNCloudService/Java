# HashMap

**1 HashMap引入**

问题：建立国家英文简称和中文全名间的键值映射，并通过key对value进行操作，应该如何实现数据的

存储和操作呢？

分析：

 Map接口专门处理键值映射数据的存储，可以根据键实现对值的操作。

最常用的实现类是HashMap。

【使用HashMap存储元素】

【Map接口常用方法】



**2 HashMa数据结构**

**1、HashMap**

 HashMap是基于哈希表的Map接口实现的，它存储的是内容是键值对<key,value>映射。此类不保证映

射的顺序，假定哈希函数将元素适当的分布在各桶之间，可为基本操作(get和put)提供稳定的性能。

在API中给出了相应的定义：

```java
//1、哈希表基于map接口的实现，这个实现提供了map所有的操作，并且提供了key和value，可以为 

null，(HashMap和HashTable大致上是一样的，除了hashmap是异步的，和允许key和value为 

null)， 

//这个类不确定map中元素的位置，特别要提的是，这个类也不确定元素的位置随着时间会不会保持不 

变。

Hash table based implementation of the Map interface. This implementation 

provides all of the optional map operations, and permits null values and the 

null key. 

(The HashMap class is roughly equivalent to Hashtable, except that it is 

unsynchronized and permits nulls.) This class makes no guarantees as to the 

order of the map; 

in particular, it does not guarantee that the order will remain constant 

over time. 

//假设哈希函数将元素合适的分到了每个桶(其实就是指的数组中位置上的链表)中，则这个实现为基本 

的操作(get、put)提供了稳定的性能，迭代这个集合视图需要的时间跟hashMap实例(key-value映射 

的数量)的容量(在桶中)成正比，因此，如果迭代的性能很重要的话，就不要将初始容量设置的太高或者 

loadfactor设置的太低，【这里的桶，相当于在数组中每个位置上放一个桶装元素】 

This implementation provides constant-time performance for the basic 

operations (get and put), assuming the hash function disperses the elements 

properly among the buckets. 

Iteration over collection views requires time proportional to the 

"capacity" of the HashMap instance (the number of buckets) plus its size 

(the number of key-value mappings 

). Thus, it's very important not to set the initial capacity too high (or 

the load factor too low) if iteration performance is important. 

//HashMap的实例有两个参数影响性能，初始化容量(initialCapacity)和loadFactor加载因子， 

在哈希表中这个容量是桶的数量【也就是数组的长度】，一个初始化容量仅仅是在哈希表被创建时容量， 

在容量自动增长之前加载因子是衡量哈希表被允许达到的多少的。当entry的数量在哈希表中超过了加载 

因子乘以当前的容量，那么哈希表被修改(内部的数据结构会被重新建立)所以哈希表有大约两倍的桶的 

数量. 

An instance of HashMap has two parameters that affect its performance: 

initial capacity and load factor. The capacity is the number of buckets in 

the hash table, 

and the initial capacity is simply the capacity at the time the hash table 

is created. The load factor is a measure of how full the hash table is 

allowed to get before 

its capacity is automatically increased. When the number of entries in the 

hash table exceeds the product of the load factor and the current capacity, 

the hash table 

is rehashed (that is, internal data structures are rebuilt) so that the hash 

table has approximately twice the number of buckets. 

//通常来讲，默认的加载因子(0.75)能够在时间和空间上提供一个好的平衡，更高的值会减少空间上的 

开支但是会增加查询花费的时间（体现在HashMap类中get、put方法上），当设置初始化容量时，应该 

考虑到map中会存放entry的数量和加载因子，以便最少次数的进行rehash操作，如果初始容量大于最 

大条目数除以加载因子，则不会发生 rehash 操作。 

As a general rule, the default load factor (.75) offers a good tradeoff 

between time and space costs. Higher values decrease the space overhead but 

increase the lookup 

cost (reflected in most of the operations of the HashMap class, including 

get and put). The expected number of entries in the map and its load factor 

should be taken 

into account when setting its initial capacity, so as to minimize the number 

of rehash operations. If the initial capacity is greater than the maximum 

number of 

entries divided by the load factor, no rehash operations will ever occur. 

//如果很多映射关系要存储在 HashMap 实例中，则相对于按需执行自动的 rehash 操作以增大表的 

容量来说，使用足够大的初始容量创建它将使得映射关系能更有效地存储。 

If many mappings are to be stored in a HashMap instance, creating it with a 

sufficiently large capacity will allow the mappings to be stored more 

efficiently than letting 

it perform automatic rehashing as needed to grow the table 
```

**2、HashMap在JDK1.8以前数据结构和存储原理**

【链表散列】

通过数组和链表结合在一起使用，就叫做链表散列。其实就是hashmap存储的原

【HashMap的数据结构和存储原理】

 HashMap的数据结构就是用的链表散列。那HashMap底层是怎么样使用这个数据结构进行数据存取的呢？分成两个部分：

第一步：HashMap内部有一个entry的内部类，其中有四个属性，我们要存储一个值，则需要一个key

和一个value，存到map中就会先将key和value保存在这个Entry类创建的对象中。

```java
static class Entry<K,V> implements Map.Entry<K,V> { 

final K key; //就是我们说的map的key 

V value; //value值，这两个都不陌生 

Entry<K,V> next;//指向下一个entry对象 

int hash;//通过key算过来的你hashcode值。 

} 
```

第二步：构造好了entry对象，然后将该对象放入数组中，如何存放就是这hashMap的精华所在了。

大概的一个存放过程是：通过entry对象中的hash值来确定将该对象存放在数组中的哪个位置上，如果

在这个位置上还有其他元素，则通过链表来存储这个元素。

【Hash存放元素的过程】

通过key、value封装成一个entry对象，然后通过key的值来计算该entry的hash值，通过entry的hash

值和数组的长度length来计算出entry放在数组中的哪个位置上面，

每次存放都是将entry放在第一个位置。在这个过程中，就是通过hash值来确定将该对象存放在数组中

的哪个位置上。



**3、JDK1.8后HashMap的数据结构**

HashMap的数据结构（数组+链表+红黑树），桶中的结构可能是链表，也可能是红

黑树，红黑树的引入是为了提高效率。

**4、HashMap的属性**

HashMap的实例有两个参数影响其性能。

初始容量：哈希表中桶的数量

加载因子：哈希表在其容量自动增加之前可以达到多满，的一种尺度

当哈希表中条目数超出了当前容量*加载因子(其实就是HashMap的实际容量)时，则对该哈希表进行

rehash操作，将哈希表扩充至两倍的桶数。

Java中默认初始容量为16，加载因子为0.75

```java
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16 

static final float DEFAULT_LOAD_FACTOR = 0.75f; 
```

**【loadFactor加载因子】**

定义：loadFactor译为装载因子。装载因子用来衡量HashMap满的程度。loadFactor的默认值为

0.75f。计算HashMap的实时装载因子的方法为：size/capacity，而不是占用桶的数量去除以capacity。

 loadFactor加载因子是控制数组存放数据的疏密程度，loadFactor越趋近于1，那么数组中存放的数据

(entry)也就越多，也就越密，也就是会让链表的长度增加，loadFactor越小，也就是趋近于0，那么数组中存放的数据也就越稀，也就是可能数组中每个位置上就放一个元素。那有人说，就把loadFactor变为1最好吗，存的数据很多，但是这样会有一个问题，就是我们在通过key拿到我们的value时，是先通过key 的hashcode值，找到对应数组中的位置，如果该位置中有很多元素，则需要通过equals来依次比较链表中的元素，拿到我们的value值，这样花费的性能就很高，如果能让数组上的每个位置尽量只有一个元素最好，我们就能直接得到value值了，所以有人又会说，那把loadFactor变得很小不就好了，但是如果变得太小，在数组中的位置就会太稀，也就是分散的太开，浪费很多空间，这样也不好，所以在hashMap中loadFactor的初始值就是0.75，一般情况下不需要更改它。

```java
static final float DEFAULT_LOAD_FACTOR = 0.75f; 
```

【桶】

根据前面画的HashMap存储的数据结构图，你这样想，数组中每一个位置上都放有一个桶，每个桶里

就是装一个链表，链表中可以有很多个元素(entry)，这就是桶的意思。也就相当于把元素都放在桶中。

【capacity】

 capacity译为容量代表的数组的容量，也就是数组的长度，同时也是HashMap中桶的个数。默认值是

16。

一般第一次扩容时会扩容到64，之后好像是2倍。总之，**容量都是2的幂。** 

```java
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16 
```

【size的含义】

 size就是在该HashMap的实例中实际存储的元素的个数

【threshold的作用】

```java
int threshold; 
```

 threshold = capacity * loadFactor，当Size>=threshold的时候，那么就要考虑对数组的扩增了，也就

是说，这个的意思就是衡量数组是否需要扩增的一个标准。

注意这里说的是考虑，因为实际上要扩增数组，除了这个size>=threshold条件外，还需要另外一个条

件。

什么时候会扩增数组的大小？在put一个元素时先size>=threshold并且还要在对应数组位置上有元素，这才能扩增数组。
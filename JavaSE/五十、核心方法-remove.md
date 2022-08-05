# 五十、核心方法-remove

这几个删除方法都是类似的。我们选择几个讲，其中fastRemove(int)方法是private的，是提供给

remove(Object)这个方法用的。

1. remove(int)：通过删除指定位置上的元素

```java
public E remove(int index) { 

rangeCheck(index);//检查index的合理性 

modCount++;//这个作用很多，比如用来检测快速失败的一种标志。 

E oldValue = elementData(index);//通过索引直接找到该元素 

int numMoved = size - index - 1;//计算要移动的位数。 

if (numMoved > 0) 

//这个方法也已经解释过了，就是用来移动元素的。 

System.arraycopy(elementData, index+1, elementData, index, 

numMoved); 

//将--size上的位置赋值为null，让gc(垃圾回收机制)更快的回收它。 

elementData[--size] = null; // clear to let GC do its work 

//返回删除的元素。 

return oldValue; 

} 
```

1. remove(Object)：这个方法可以看出来，arrayList是可以存放null值得。

```java
//就是通过元素来删除该元素，就依次遍历，如果有这个元素， 

就将该元素的索引传给fastRemobe(index)，使用这个方法来删除该元素， 

//fastRemove(index)方法的内部跟remove(index)的实现几乎一样，这里最主要是知道 

arrayList可以存储null值 

public boolean remove(Object o) { 

if (o == null) { 

for (int index = 0; index < size; index++) 

if (elementData[index] == null) { 

fastRemove(index); 

return true; 

} 

} else { 

for (int index = 0; index < size; index++) 

if (o.equals(elementData[index])) { 

fastRemove(index); 

return true; 

} 

}

return false; 

} 
```

1. clear()：将elementData中每个元素都赋值为null，等待垃圾回收将这个给回收掉，所以叫clear

```java
public void clear() { 

modCount++; 

// clear to let GC do its work 

for (int i = 0; i < size; i++) 

elementData[i] = null; 

size = 0; 

} 
```

1. removeAll(collection c)

```java
public boolean removeAll(Collection<?> c) { 

return batchRemove(c, false);//批量删除 

} 
```

1. batchRemove(xx,xx)：用于两个方法，一个removeAll()：它只清楚指定集合中的元素，retainAll()用来测试两个集合是否有交集。

```java
//这个方法，用于两处地方，如果complement为false，则用于removeAll如果为true，则给 

retainAll()用，retainAll（）是用来检测两个集合是否有交集的。 

private boolean batchRemove(Collection<?> c, boolean complement) { 

final Object[] elementData = this.elementData; //将原集合，记名为A 

int r = 0, w = 0; //r用来控制循环，w是记录有多少个交集

boolean modified = false; 

try {

for (; r < size; r++) 

//参数中的集合C一次检测集合A中的元素是否有， 

if (c.contains(elementData[r]) == complement) 

//有的话，就给集合A 

elementData[w++] = elementData[r]; 

} finally { 

// Preserve behavioral compatibility with AbstractCollection, 

// even if c.contains() throws. 

//如果contains方法使用过程报异常 

if (r != size) { 

//将剩下的元素都赋值给集合A， 

System.arraycopy(elementData, r, 

elementData, w, 

size - r); 

w += size - r; 

}

if (w != size) { 

//这里有两个用途，在removeAll()时，w一直为0，就直接跟clear一样，全是为 

null。 

//retainAll()：没有一个交集返回true，有交集但不全交也返回true，而两个集合 

相等的时候，返回false，所以不能根据返回值来确认两个集合是否有交集，而是通过原集合的大小是否 

发生改变来判断，如果原集合中还有元素，则代表有交集，而元集合没有元素了，说明两个集合没有交 

集。 

// clear to let GC do its work 

for (int i = w; i < size; i++) 

elementData[i] = null; 

modCount += size - w; 

size = w; 

modified = true; 

} 

}

return modified; 

} 
```

总结：remove函数，用户移除指定下标的元素，此时会把指定下标到数组末尾的元素向前移动一个单

位，并且会把数组最后一个元素设置为null，这样是为了方便之后将整个数组不被使用时，会被GC，可以作为小的技巧使用。
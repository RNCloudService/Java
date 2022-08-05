# 四十九、核心方法-add

1. **boolean add(E)**

```java
/** 

\* Appends the specified element to the end of this list. 

\* 添加一个特定的元素到list的末尾。 

\* @param e element to be appended to this list 

\* @return <tt>true</tt> (as specified by {@link Collection#add}) 

*/ 

public boolean add(E e) { 

//确定内部容量是否够了，size是数组中数据的个数，因为要添加一个元素，所以size+1，先判 

断size+1的这个个数数组能否放得下，就在这个方法中去判断是否数组.length是否够用了。 

ensureCapacityInternal(size + 1); // Increments modCount!! 

elementData[size++] = e; //在数据中正确的位置上放上元素e，并且size++ 

return true; 

} 
```

【分析：ensureCapacityInternal(xxx); 确定内部容量的方法】

```java
private void ensureCapacityInternal(int minCapacity) { 

ensureExplicitCapacity(calculateCapacity(elementData, minCapacity)); 

}

private static int calculateCapacity(Object[] elementData, int minCapacity) 

{ 

//看，判断初始化的elementData是不是空的数组，也就是没有长度 

if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) { 

//因为如果是空的话，minCapacity=size+1；其实就是等于1，空的数组没有长度就存 

放不了，所以就将minCapacity变成10，也就是默认大小，但是在这里，还没有真正的初始化这个 

elementData的大小。 

return Math.max(DEFAULT_CAPACITY, minCapacity); 

}

//确认实际的容量，上面只是将minCapacity=10，这个方法就是真正的判断elementData是否 

够用 

return minCapacity; 

}

private void ensureExplicitCapacity(int minCapacity) { 

modCount++; 

// overflow-conscious code 

//minCapacity如果大于了实际elementData的长度，那么就说明elementData数组的长度不 

够用，不够用那么就要增加elementData的length。这里有的同学就会模糊minCapacity到底是什么 

呢，这里给你们分析一下 

/*第一种情况：由于elementData初始化时是空的数组，那么第一次add的时候， 

minCapacity=size+1；也就minCapacity=1，在上一个方法(确定内部容量 

ensureCapacityInternal)就会判断出是空的数组，就会给将minCapacity=10，到这一步为止， 

还没有改变elementData的大小。 

第二种情况：elementData不是空的数组了，那么在add的时候，minCapacity=size+1；也就是 

minCapacity代表着elementData中增加之后的实际数据个数，拿着它判断elementData的length 

是否够用，如果length 

不够用，那么肯定要扩大容量，不然增加的这个元素就会溢出。*/ 

if (minCapacity - elementData.length > 0) 

grow(minCapacity); 

}

//arrayList核心的方法，能扩展数组大小的真正秘密。 

private void grow(int minCapacity) { 

// overflow-conscious code 

//将扩充前的elementData大小给oldCapacity 

int oldCapacity = elementData.length; 

//newCapacity就是1.5倍的oldCapacity 

int newCapacity = oldCapacity + (oldCapacity >> 1); 

//这句话就是适应于elementData就空数组的时候，length=0，那么oldCapacity=0， 

newCapacity=0，所以这个判断成立，在这里就是真正的初始化elementData的大小了，就是为10. 

前面的工作都是准备工作。 

if (newCapacity - minCapacity < 0) 

newCapacity = minCapacity; 

//如果newCapacity超过了最大的容量限制，就调用hugeCapacity，也就是将能给的最大值给 

newCapacity 

if (newCapacity - MAX_ARRAY_SIZE > 0) 

newCapacity = hugeCapacity(minCapacity); 

// minCapacity is usually close to size, so this is a win: 

//新的容量大小已经确定好了，就copy数组，改变容量大小咯。 

elementData = Arrays.copyOf(elementData, newCapacity); 

}

//这个就是上面用到的方法，很简单，就是用来赋最大值。 

private static int hugeCapacity(int minCapacity) { 

if (minCapacity < 0) // overflow 

throw new OutOfMemoryError(); 

//如果minCapacity都大于MAX_ARRAY_SIZE，那么就Integer.MAX_VALUE返回，反之将 

MAX_ARRAY_SIZE返回。因为maxCapacity是三倍的minCapacity，可能扩充的太大了，就用 

minCapacity来判断了。 

//Integer.MAX_VALUE:2147483647 MAX_ARRAY_SIZE：2147483639 也就是说最大也就能 

给到第一个数值。还是超过了这个限制，就要溢出了。相当于arraylist给了两层防护。 

return (minCapacity > MAX_ARRAY_SIZE) ? 

Integer.MAX_VALUE : 

MAX_ARRAY_SIZE; 

} 
```

1. **void add(int，E)**

```java
public void add(int index, E element) { 

//检查index也就是插入的位置是否合理。 

rangeCheckForAdd(index); 

ensureCapacityInternal(size + 1); // Increments modCount!! 

//这个方法就是用来在插入元素之后，要将index之后的元素都往后移一位， 

System.arraycopy(elementData, index, elementData, index + 1, 

size - index); 

//在目标位置上存放元素 

elementData[index] = element; 

size++; 

} 
```

【分析：rangeCheckForAdd(index)】 

```java
private void rangeCheckForAdd(int index) { 

//插入的位置肯定不能大于size 和小于0 

if (index > size || index < 0) 

//如果是，就报这个越界异常 

throw new IndexOutOfBoundsException(outOfBoundsMsg(index)); 

} 
```

【System.arraycopy(...)：就是将elementData在插入位置后的所有元素，往后面移一位.】 

```java
public static void arraycopy(Object src, 

int srcPos, 

Object dest, 

int destPos, 

int length) 

src：源对象 

srcPos：源对象对象的起始位置 

dest：目标对象 

destPost：目标对象的起始位置 

length：从起始位置往后复制的长度。 

//这段的大概意思就是解释这个方法的用法，复制src到dest，复制的位置是从src的srcPost开始， 

到srcPost+length-1的位置结束，复制到destPost上，从destPost开始到destPost+length-1 

的位置上， 

Copies an array from the specified source array, beginning at the specified 

position, to the specified position of the destination array. A subsequence 

of array components are copied from 

the source array referenced by src to the destination array referenced by 

dest. The number of components copied is equal to the length argument. The 

components at positions srcPos through srcPos+length-1 

in the source array are copied into positions destPos through 

destPos+length-1, respectively, of the destination array. 

//告诉你复制的一种情况，如果A和B是一样的，那么先将A复制到临时数组C，然后通过C复制到B，用了 

一个第三方参数 

If the src and dest arguments refer to the same array object, then the 

copying is performed as if the components at positions srcPos through 

srcPos+length-1 were first copied to 

a temporary array with length components and then the contents of the 

temporary array were copied into positions destPos through destPos+length-1 

of the destination array. 

//这一大段，就是来说明会出现的一些问题，NullPointerException和 

IndexOutOfBoundsException 还有ArrayStoreException 这三个异常出现的原因。 

If dest is null, then a NullPointerException is thrown. 

If src is null, then a NullPointerException is thrown and the destination 

array is not modified. 

Otherwise, if any of the following is true, an ArrayStoreException is thrown 

and the destination is not modified: 

The src argument refers to an object that is not an array. 

The dest argument refers to an object that is not an array. 

The src argument and dest argument refer to arrays whose component types are 

different primitive types. 

The src argument refers to an array with a primitive component type and the 

dest argument refers to an array with a reference component type. 

The src argument refers to an array with a reference component type and the 

dest argument refers to an array with a primitive component type. 

Otherwise, if any of the following is true, an IndexOutOfBoundsException is 

thrown and the destination is not modified: 

The srcPos argument is negative. 

The destPos argument is negative. 

The length argument is negative. 

srcPos+length is greater than src.length, the length of the source array. 

destPos+length is greater than dest.length, the length of the destination 

array. 

//这里描述了一种特殊的情况，就是当A的长度大于B的长度的时候，会复制一部分，而不是完全失败。 

Otherwise, if any actual component of the source array from position srcPos 

through srcPos+length-1 cannot be converted to the component type of the 

destination array by assignment conversion, an ArrayStoreException is 

thrown. 

In this case, let k be the smallest nonnegative integer less than length 

such that src[srcPos+k] cannot be converted to the component type of the 

destination array; when the exception is thrown, source array components 

from positions 

srcPos through srcPos+k-1 will already have been copied to destination array 

positions destPos through destPos+k-1 and no other positions of the 

destination array will have been modified. (Because of the restrictions 

already itemized, 

this paragraph effectively applies only to the situation where both arrays 

have component types that are reference types.) 

//这个参数列表的解释，一开始就说了， 

Parameters: 

src - the source array. 

srcPos - starting position in the source array. 

dest - the destination array. 

destPos - starting position in the destination data. 

length - the number of array elements to be copied. 
```

【**总结**】

正常情况下会扩容1.5倍，特殊情况下（新扩展数组大小已经达到了最大值）则只取最大值。

程序调用add，实际上还会进行一系列调用，可能会调用到grow，grow可能会调用

hugeCapacity

【举例】

```java
List<Integer> lists = new ArrayList<Integer>; 

lists.add(8); 
```

说明：在add方法之前开始elementData = {}；调用add方法时会继续调用，直至

grow，最后elementData的大小变为10，之后再返回到add函数，把8放在elementData[0]中。

【举例说明二】

```java
List<Integer> lists = new ArrayList<Integer>(6); 

lists.add(8); 
```

说明：我们可以知道，在调用add方法之前，elementData的大小已经为6，之后再进行传递，不会进行扩容处理。
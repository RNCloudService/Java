# **Arrays** **类**

数组的工具类java.util.Arrays

由于数组对象本身并没有什么方法可以供我们调用,但API中提供了一个工具类Arrays供我们使用,从

而可以对数据对象进行一些基本的操作。

Arrays类中的方法都是static修饰的静态方法,在使用的时候可以直接使用类名进行调用,而"不用"使用对

象来调用(注意:是"不用" 而不是 "不能")

java.util.Arrays 类能方便地操作数组. 使用之前需要导包！

具有以下常用功能：

给数组赋值：通过 fifill 方法。

对数组排序：通过 sort 方法,按升序。

比较数组：通过 equals 方法比较数组中元素值是否相等。

查找数组元素：通过 binarySearch 方法能对排序好的数组进行二分查找法操作。

### 1、打印数组 

```java
public static void main(String[] args) { 

int[] a = {1,2}; 

System.out.println(a); //[I@1b6d3586 

System.out.println(Arrays.toString(a)); //[1, 2] 

} 
```

### 2、数组排序

对指定的 int 型数组按数字升序进行排序

```java
public static void main(String[] args) { 

int[] a = {1,2,323,23,543,12,59}; 

System.out.println(Arrays.toString(a)); 

Arrays.sort(a); 

System.out.println(Arrays.toString(a)); 

} 
```

### 3、二分法查找

在数组中查找指定元素并返回其下标

使用二分搜索法来搜索指定的数组，以获得指定的值。必须在进行此调用之前对数组进行排序(通 

过sort方法等)。如果没有对数组进行排序，则结果是不确定的。

如果数组包含多个带有指定值的元素，则无法保证找到的是哪一个。

```java
public static void main(String[] args) { 

int[] a = {1,2,323,23,543,12,59}; 

Arrays.sort(a); //使用二分法查找，必须先对数组进行排序 

System.out.println("该元素的索引："+Arrays.binarySearch(a, 12)); 

} 
```

### 4、元素填充

```java
public static void main(String[] args) { 

int[] a = {1,2,323,23,543,12,59}; 

Arrays.sort(a); //使用二分法查找，必须先对数组进行排序 

Arrays.fill(a, 2, 4, 100); //将2到4索引的元素替换为100 

System.out.println(Arrays.toString(a)); 

} 
```

### 5、数组转换为Lis集合 

```java
int[] a = {3,5,1,9,7}; 

List<int[]> list = Arrays.asList(a); 
```


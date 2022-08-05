# 五十八、Collections工具类

Java提供了一个操作Set、List和Map等集合的工具类：Collections，该工具类提供了大量方法对集合进

行排序、查询和修改等操作，还提供了将集合对象置为不可变、对集合对象实现同步控制等方法。

这个类不需要创建对象，内部提供的都是静态方法。

**1、Collectios概述**

此类完全由在 collection 上进行操作或返回 collection 的静态方法组成。它包含在 collection 上操作的

多态算法，即“包装器”，包装器返回由指定 collection 支持的新 collection，以及少数其他内容。如果为此类的方法所提供的 collection 或类对象为 null，则这些方法都将抛出 `NullPointerException` 。 

**2、排序操作**

【方法】

```java
1）static void reverse(List<?> list): 

反转列表中元素的顺序。 

2）static void shuffle(List<?> list) : 

对List集合元素进行随机排序。 

3） static void sort(List<T> list) 

根据元素的自然顺序 对指定列表按升序进行排序 

4）static <T> void sort(List<T> list, Comparator<? super T> c) : 

根据指定比较器产生的顺序对指定列表进行排序。 

5）static void swap(List<?> list, int i, int j) 

在指定List的指定位置i,j处交换元素。 

6）static void rotate(List<?> list, int distance) 

当distance为正数时，将List集合的后distance个元素“整体”移到前面；当distance为 

负数时，将list集合的前distance个元素“整体”移到后边。该方法不会改变集合的长度。
```

【演示】

```java
import java.util.ArrayList; 

import java.util.Collections; 

public class CollectionsTest { 

public static void main(String[] args) { 

ArrayList list = new ArrayList(); 

list.add(3); 

list.add(-2); 

list.add(9); 

list.add(5); 

list.add(-1); 

list.add(6); 

//输出：[3, -2, 9, 5, -1, 6] 

System.out.println(list); 

//集合元素的次序反转 

Collections.reverse(list); 

//输出：[6, -1, 5, 9, -2, 3] 

System.out.println(list); 

//排序：按照升序排序 

Collections.sort(list); 

//[-2, -1, 3, 5, 6, 9] 

System.out.println(list); 

//根据下标进行交换 

Collections.swap(list, 2, 5); 

//输出：[-2, -1, 9, 5, 6, 3] 

System.out.println(list); 

/*//随机排序 

Collections.shuffle(list); 

//每次输出的次序不固定 

System.out.println(list);*/ 

//后两个整体移动到前边 

Collections.rotate(list, 2); 

//输出：[6, 9, -2, -1, 3, 5] 

System.out.println(list); 

} 

} 
```

**3、查找、替换操作**

【方法】

```java
1） static <T> int binarySearch(List<? extends Comparable<? super T>> 

list, T key) 

使用二分搜索法搜索指定列表，以获得指定对象在List集合中的索引。 

注意：此前必须保证List集合中的元素已经处于有序状态。 

2）static Object max(Collection coll) 

根据元素的自然顺序，返回给定collection 的最大元素。 

3）static Object max(Collection coll,Comparator comp): 

根据指定比较器产生的顺序，返回给定 collection 的最大元素。 

4）static Object min(Collection coll): 

根据元素的自然顺序，返回给定collection 的最小元素。 

5）static Object min(Collection coll,Comparator comp): 

根据指定比较器产生的顺序，返回给定 collection 的最小元素。 

6） static <T> void fill(List<? super T> list, T obj) : 

使用指定元素替换指定列表中的所有元素。 

7）static int frequency(Collection<?> c, Object o) 

返回指定 collection 中等于指定对象的出现次数。 

8）static int indexOfSubList(List<?> source, List<?> target) : 

返回指定源列表中第一次出现指定目标列表的起始位置；如果没有出现这样的列表，则返回 

-1。

9）static int lastIndexOfSubList(List<?> source, List<?> target) 

返回指定源列表中最后一次出现指定目标列表的起始位置；如果没有出现这样的列表，则返回 

-1。

10）static <T> boolean replaceAll(List<T> list, T oldVal, T newVal) 

使用一个新值替换List对象的所有旧值oldVal 
```

【演示：实例使用查找、替换操作】

```java
import java.util.ArrayList; 

import java.util.Collections; 

public class CollectionsTest1 { 

public static void main(String[] args) { 

ArrayList list = new ArrayList(); 

list.add(3); 

list.add(-2); 

list.add(9); 

list.add(5); 

list.add(-1); 

list.add(6); 

//[3, -2, 9, 5, -1, 6] 

System.out.println(list); 

//输出最大元素9 

System.out.println(Collections.max(list)); 

//输出最小元素：-2 

System.out.println(Collections.min(list)); 

//将list中的-2用1来代替 

System.out.println(Collections.replaceAll(list, -2, 1)); 

//[3, 1, 9, 5, -1, 6] 

System.out.println(list); 

list.add(9); 

//判断9在集合中出现的次数，返回2 

System.out.println(Collections.frequency(list, 9)); 

//对集合进行排序 

Collections.sort(list); 

//[-1, 1, 3, 5, 6, 9, 9] 

System.out.println(list); 

//只有排序后的List集合才可用二分法查询，输出2 

System.out.println(Collections.binarySearch(list, 3));

} 

} 
```

**4、同步控制**

Collectons提供了多个synchronizedXxx()方法·，该方法可以将指定集合包装成线程同步的集合，从

而解决多线程并发访问集合时的线程安全问题。

正如前面介绍的HashSet，TreeSet，arrayList,LinkedList,HashMap,TreeMap都是线程不安全的。

Collections提供了多个静态方法可以把他们包装成线程同步的集合。

【方法】

```java
1）static <T> Collection<T> synchronizedCollection(Collection<T> c) 

返回指定 collection 支持的同步（线程安全的）collection。 

2）static <T> List<T> synchronizedList(List<T> list) 

返回指定列表支持的同步（线程安全的）列表。 

3）static <K,V> Map<K,V> synchronizedMap(Map<K,V> m) 

返回由指定映射支持的同步（线程安全的）映射。 

4）static <T> Set<T> synchronizedSet(Set<T> s) 

返回指定 set 支持的同步（线程安全的）set。
```

【实例】

```java
import java.util.*; 

public class TestSynchronized 

{ 

public static void main(String[] args) 

{ 

//下面程序创建了四个同步的集合对象 

Collection c = Collections.synchronizedCollection(new ArrayList()); 

List list = Collections.synchronizedList(new ArrayList()); 

Set s = Collections.synchronizedSet(new HashSet()); 

Map m = Collections.synchronizedMap(new HashMap()); 

} 

} 
```

**5、Collesction设置不可变集合**

【方法】

```java
1）emptyXxx() 

返回一个空的、不可变的集合对象，此处的集合既可以是List，也可以是Set，还可以是 

Map。

2）singletonXxx(): 

返回一个只包含指定对象（只有一个或一个元素）的不可变的集合对象，此处的集合可以是： 

List，Set，Map。 

3）unmodifiableXxx(): 

返回指定集合对象的不可变视图，此处的集合可以是：List，Set，Map。 
```

上面三类方法的参数是原有的集合对象，返回值是该集合的”只读“版本。

【实例】

```java
import java.util.*; 

public class TestUnmodifiable 

{ 

public static void main(String[] args) 

{ 

//创建一个空的、不可改变的List对象 

List<String> unmodifiableList = Collections.emptyList(); 

//unmodifiableList.add("java"); 

//添加出现异常：java.lang.UnsupportedOperationException 

System.out.println(unmodifiableList);// [] 

//创建一个只有一个元素，且不可改变的Set对象 

Set unmodifiableSet = Collections.singleton("Struts2权威指南"); 

//[Struts2权威指南] 

System.out.println(unmodifiableSet); 

//创建一个普通Map对象 

Map scores = new HashMap(); 

scores.put("语文" , 80); 

scores.put("Java" , 82); 

//返回普通Map对象对应的不可变版本 

Map unmodifiableMap = Collections.unmodifiableMap(scores); 

//下面任意一行代码都将引发UnsupportedOperationException异常 

unmodifiableList.add("测试元素"); 

unmodifiableSet.add("测试元素"); 

unmodifiableMap.put("语文",90); 

} 

} 
```

**总结和测试** 

【JavaBean】

实体类：Pojo

```java
import java.text.DateFormat; 

import java.text.ParseException; 

import java.text.SimpleDateFormat; 

import java.util.Date; 

public class Employee { //Javabean, Enter实体类 

private int id; 

private String name; 

private int salary; 

private String department; 

private Date hireDate; 

public Employee(int id, String name, int salary, String department, 

String hireDate) { 

super(); 

this.id = id; 

this.name = name; 

this.salary = salary; 

this.department = department; 

DateFormat format = new SimpleDateFormat("yyyy-MM"); 

try {

this.hireDate = format.parse(hireDate); 

} catch (ParseException e) { 

e.printStackTrace(); 

} 

}

public int getId() { 

return id; 

}

public void setId(int id) { 

this.id = id; 

}

public String getName() { 

return name; 

}

public void setName(String name) { 

this.name = name; 

}

public int getSalary() { 

return salary; 

}

public void setSalary(int salary) { 

this.salary = salary; 

}

public String getDepartment() { 

return department; 

}

public void setDepartment(String department) { 

this.department = department; 

}

public Date getHireDate() { 

return hireDate; 

}

public void setHireDate(Date hireDate) { 

this.hireDate = hireDate; 

} 

} 
```

【测试类代码如下】

```java
import java.util.ArrayList; 

import java.util.List; 

public class Test01 { 

public static void main(String[] args) throws Exception { 

//一个对象对应了一行记录！ 

Employee e = new Employee(0301,"狂神",3000,"项目部","2017-10"); 

Employee e2 = new Employee(0302,"小明",3500,"教学部","2016-10"); 

Employee e3 = new Employee(0303,"小红",3550,"教学部","2016-10"); 

List<Employee> list = new ArrayList<Employee>(); 

list.add(e); 

list.add(e2); 

list.add(e3); 

printEmpName(list); 

}

public static void printEmpName(List<Employee> list){ 

for(int i=0;i<list.size();i++){ 

System.out.println(list.get(i).getName()); 

} 

} 

}
```


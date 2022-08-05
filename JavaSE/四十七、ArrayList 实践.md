# 四十七、**ArrayList** **实践**

问题：我们现在有4只小狗，我们如何存储它的信息，获取总数，并能够逐条打印狗狗信息！

分析：通过List 接口的实现类ArrayList 实现该需求.

元素个数不确定

要求获得元素的实际个数

按照存储顺序获取并打印元素信息

```java
class Dog { 

private String name; 

//构造。。。set、get、。。。toString（） 

}

public class TestArrayList { 

public static void main(String[] args) { 

//创建ArrayList对象 , 并存储狗狗 

List dogs = new ArrayList(); 

dogs.add(new Dog("小狗一号")); 

dogs.add(new Dog("小狗二号")); 

dogs.add(new Dog("小狗三号")); 

dogs.add(2,new Dog("小狗四号"));// 添加到指定位置 

// .size() ： ArrayList大小 

System.out.println("共计有" + dogs.size() + "条狗狗。"); 

System.out.println("分别是："); 

// .get(i) ： 逐个获取个元素 

for (int i = 0; i < dogs.size(); i++) { 

Dog dog = (Dog) dogs.get(i); 

System.out.println(dog.getName()); 

} 

} 

} 
```

问题联想：

删除第一个狗狗 ：remove（index）

删除指定位置的狗狗 ：remove（object）

判断集合中是否包含指定狗狗 ： contains（object）

分析：使用List接口提供的remove()、contains()方法
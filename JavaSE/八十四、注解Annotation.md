# 八十四、注解Annotation

### **1、什么是注解**

- Annotation 是从JDK5.0开始引入的新技术 .
- Annotation的作用

- [ ] 1. 不是程序本身 , 可以对程序作出解释.(这一点和注释(comment)没什么区别)
  2. 可以被其他程序(比如:编译器等)读取.
- [ ] - Annotation的格式

- [ ] 注解是以"@注释名"在代码中存在的
- [ ] 还可以添加一些参数值 , 例如:@SuppressWarnings(value="unchecked")

- Annotation在哪里使用?

- [ ] 可以附加在package , class , method , fifield 等上面 , 相当于给他们添加了额外的辅助信息
- [ ] 我们可以通过反射机制实现对这些元数据的访问

### 2、内置注解

- @Override

- [ ] 定义在 java.lang.Override 中 , 此注释只适用于修辞方法 , 表示一个方法声明打算重写超类中

  的另一个方法声明.

- @Deprecated

- [ ] 定义在java.lang.Deprecated中 , 此注释可以用于修辞方法 , 属性 , 类 ,
- [ ] 表示不鼓励程序员使用这样的元素 , 通常是因为它很危险或者存在更好的选择 .

- @SuppressWarnings

- [ ] 定义在java.lang.SuppressWarnings中,用来抑制编译时的警告信息.

- [ ] 与前两个注释有所不同,你需要添加一个参数才能正确使用,这些参数都是已经定义好了的,我们

  选择性的使用就好了 .

1. @SuppressWarnings("all")
2. @SuppressWarnings("unchecked")
3. @SuppressWarnings(value={"unchecked","deprecation"})
4. 等等 .....

```java
package com.annotation; 

//测试内置注解 

import java.util.ArrayList; 

import java.util.List; 

//所有类默认继承Object类 

public class Test1 extends Object { 

//@Override 表示方法重写 

//--> 查看JDK帮助文档 

//--> 测试名字不同产生的效果 

@Override 

public String toString() { 

return super.toString(); 

}

//方法过时了, 不建议使用 , 可能存在问题 , 并不是不能使用! 

//--> 查看JDK帮助文档 

@Deprecated 

public static void stop(){ 

System.out.println("测试 @Deprecated"); 

}

//@SuppressWarnings 抑制警告 , 可以传参数 

//--> 查看JDK帮助文档 

//查看源码:发现 参数类型 和 参数名称 , 并不是方法! 

@SuppressWarnings("all") 

public void sw(){ 

List list = new ArrayList(); 

}

public static void main(String[] args) { 

stop(); 

} 

} 
```

### 3、元注解

- 元注解的作用就是负责注解其他注解 , Java定义了4个标准的meta-annotation类型,他们被用来提供

  对其他annotation类型作说明 .

- 这些类型和它们所支持的类在java.lang.annotation包中可以找到 .( @Target , @Retention ,

  @Documented , @Inherited )

- [ ] @Target : 用于描述注解的使用范围(即:被描述的注解可以用在什么地方)
- [ ] @Retention : 表示需要在什么级别保存该注释信息 , 用于描述注解的生命周期
- [ ] (SOURCE < CLASS < RUNTIME)
- [ ] @Document：说明该注解将被包含在javadoc中
- [ ] @Inherited：说明子类可以继承父类中的该注解

```java
package com.annotation; 

import java.lang.annotation.*; 

//测试元注解 

public class Test2 { 

@MyAnnotation 

public void test(){ 

} 

}

//定义一个注解 

@Target(value = {ElementType.METHOD,ElementType.TYPE}) 

@Retention(value = RetentionPolicy.RUNTIME) 

@Inherited 

@Documented 

@interface MyAnnotation{ 

//测试作用域 , 了解@Retention的概念 

} 
```

### 4、自定义注解

- 使用 @interface自定义注解时 , 自动继承了java.lang.annotation.Annotation接口
- 分析 :

1. @ interface用来声明一个注解 , 格式 : public @ interface 注解名 { 定义内容 }
2. 其中的每一个方法实际上是声明了一个配置参数.
3. 方法的名称就是参数的名称.
4. 返回值类型就是参数的类型 ( 返回值只能是基本类型,Class , String , enum ).
5. 可以通过default来声明参数的默认值
6. 如果只有一个参数成员 , 一般参数名为value
7. 注解元素必须要有值 , 我们定义注解元素时 , 经常使用空字符串,0作为默认值 .

```java
package com.annotation; 

import java.lang.annotation.ElementType; 

import java.lang.annotation.Retention; 

import java.lang.annotation.RetentionPolicy; 

import java.lang.annotation.Target; 

//测试自定义注解 

public class Test3 { 

//显示定义值 / 不显示值就是默认值 

@MyAnnotation2(age = 18,name = "秦疆",id = 001,schools = {"西工大"}) 

public void test() { 

}

//只有一个参数, 默认名字一般是value.使用可省略不写 

@MyAnnotation3("aaa") 

public void test2(){ 

} 

}

@Target(value = {ElementType.METHOD}) 

@Retention(value = RetentionPolicy.RUNTIME) 

@interface MyAnnotation2{ 

//参数类型 , 参数名 

String name() default ""; 

int age() default 0; 

int id() default -1; //String indexOf("abc") -1 , 不存在,找不到 

String[] schools() default {"西部开源","狂神说Java"}; 

}

@Target(value = {ElementType.METHOD}) 

@Retention(value = RetentionPolicy.RUNTIME) 

@interface MyAnnotation3{ 

// 参数类型 参数名称 

String value(); 

} 
```

### 5、反射读取注解

```java
package com.annotation; 

import java.lang.annotation.*; 

import java.lang.reflect.Field; 

//测试ORM:对象关系映射 

//使用反射读取注解信息三步: 

// 1.定义注解 , 

// 2.在类中使用注解 , 

// 3. 使用反射获取注解 , 一般都是现成框架实现 , 我们手动实现 

public class Test4 { 

public static void main(String[] args) { 

try {

//反射 , Class可以获得类的全部信息 , 所有的东西 

Class clazz = Class.forName("com.annotation.Student"); 

//获得这个类的注解 

Annotation[] annotations = clazz.getAnnotations(); 

for (Annotation annotation:annotations){ 

System.out.println(annotation); 

}

//获得类的注解value的值 

TableKuang table = (TableKuang) 

clazz.getAnnotation(TableKuang.class); 

System.out.println(table.value()); 

//获得类指定注解的值 

Field name = clazz.getDeclaredField("name"); 

FieldKuang fieldKuang = name.getAnnotation(FieldKuang.class); 

System.out.println(fieldKuang.columnName()+"-- 

\>"+fieldKuang.type() 

+"-->"+fieldKuang.length()); 

//我们可以根据得到的类的信息 , 通过JDBC生成相关的SQL语句,执行就可以动态生 

成数据库表

} catch (ClassNotFoundException e) { 

e.printStackTrace(); 

} catch (NoSuchFieldException e) { 

e.printStackTrace(); 

} 

} 

}

@TableKuang("db_student") //假设数据库表名为db_student . 

class Student{ 

@FieldKuang(columnName = "db_id",type="int",length = 10) 

private int id; 

@FieldKuang(columnName = "db_name",type="varchar",length = 10) 

private String name; 

@FieldKuang(columnName = "db_age",type="int",length = 3) 

private int age; 

public Student() { 

}

public Student(int id, String name, int age) { 

this.id = id; 

this.name = name; 

this.age = age; 

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

public int getAge() { 

return age; 

}

public void setAge(int age) { 

this.age = age; 

}

@Override 

public String toString() { 

return "Student{" + 

"id=" + id + 

", name='" + name + '\'' + 

", age=" + age + 

'}'; 

} 

}

//表名注解 , 只有一个参数 , 建议使用value命名 

@Target(value = {ElementType.TYPE}) 

@Retention(value = RetentionPolicy.RUNTIME) 

@interface TableKuang{ 

String value(); 

}

//属性注解 

@Target(value = {ElementType.FIELD}) //注意字段 

@Retention(value = RetentionPolicy.RUNTIME) 

@interface FieldKuang{ 

String columnName(); //列名 

String type(); //类型 

int length();//长度 

}
```


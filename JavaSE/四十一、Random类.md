# Random类

Java中存在着两种Random函数

#### 一、java.lang.Math.Random;

调用这个Math.Random()函数能够返回带正号的double值，该值大于等于0.0且小于1.0，即取值范围是

[0.0,1.0)的左闭右开区间，返回值是一个伪随机选择的数，在该范围内（近似）均匀分布。

```java
public static void main(String[] args) { 

// 结果是个double类型的值，区间为[0.0,1.0） 

System.out.println("Math.random()=" + Math.random()); 

int num = (int) (Math.random() * 3); 

// 注意不要写成(int)Math.random()*3，这个结果为0或1，因为先执行了强制转换 

System.out.println("num=" + num); 

}

//结果 

//Math.random()=0.44938147153848396 

//num=1 
```

### 二、java.util.Random

下面是Random()的两种构造方法：

 Random()：创建一个新的随机数生成器。

 Random(long seed)：使用单个 long 种子创建一个新的随机数生成器。

你在创建一个Random对象的时候可以给定任意一个合法的种子数，种子数只是随机算法的起源数字，

和生成的随机数的区间没有任何关系。

如下面的Java代码：

【演示一】

在没带参数构造函数生成的Random对象的种子缺省是当前系统时间的毫秒数。

rand.nextInt(100)中的100是随机数的上限,产生的随机数为0-100的整数,不包括100

```java
public static void main(String[] args) { 

Random rand =new Random(); 

int i=rand.nextInt(100); 

System.out.println(i); 

} 
```

【演示二】

对于种子相同的Random对象，生成的随机数序列是一样的。

```java
public static void main(String[] args) { 

Random ran1 = new Random(25); 

System.out.println("使用种子为25的Random对象生成[0,100)内随机整数序列: "); 

for (int i = 0; i < 10; i++) { 

System.out.print(ran1.nextInt(100) + " "); 

}

System.out.println(); 

} 
```

【方法摘要】

1. protected int next(int bits)：生成下一个伪随机数。

2. boolean nextBoolean()：返回下一个伪随机数，它是取自此随机数生成器序列的均匀分布的

boolean值。

3. void nextBytes(byte[] bytes)：生成随机字节并将其置于用户提供的 byte 数组中。

4. double nextDouble()：返回下一个伪随机数，它是取自此随机数生成器序列的、在0.0和1.0之间

均匀分布的 double值。

5. flfloat nextFloat()：返回下一个伪随机数，它是取自此随机数生成器序列的、在0.0和1.0之间均匀分

布flfloat值。

6. double nextGaussian()：返回下一个伪随机数，它是取自此随机数生成器序列的、呈高斯（

“正 

态”）分布的double值，其平均值是0.0标准差是1.0。 

7. int nextInt()：返回下一个伪随机数，它是此随机数生成器的序列中均匀分布的 int 值。

8. int nextInt(int n)：返回一个伪随机数，它是取自此随机数生成器序列的、在（包括和指定值（不

包括）之间均匀分布的int值。

9. long nextLong()：返回下一个伪随机数，它是取自此随机数生成器序列的均匀分布的 long 值。

10. void setSeed(long seed)：使用单个 long 种子设置此随机数生成器的种子。

【例子】

1. 生成[0,1.0)区间的小数：double d1 = r.nextDouble();

2. 生成[0,5.0)区间的小数：double d2 = r.nextDouble() * 5;

3. 生成[1,2.5)区间的小数：double d3 = r.nextDouble() * 1.5 + 1;

4. 生成[0,10)区间的整数：int n2 = r.nextInt(10);
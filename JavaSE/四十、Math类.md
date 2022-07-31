# Math类

Java 的 Math 包含了用于执行基本数学运算的属性和方法，如初等指数、对数、平方根和三角函数。

Math 的方法都被定义为 static 形式，通过 Math 类可以在主函数中直接调用。

```java
public final class Math{ 

//数学方法 

} 
```

【常用值与函数】

```java
Math.PI 记录的圆周率

Math.E 记录e的常量

Math中还有一些类似的常量，都是一些工程数学常用量。

Math.abs 求绝对值

Math.sin 正弦函数 Math.asin 反正弦函数

Math.cos 余弦函数 Math.acos 反余弦函数

Math.tan 正切函数 Math.atan 反正切函数 Math.atan2 商的反正切函数

Math.toDegrees 弧度转化为角度 Math.toRadians 角度转化为弧度

Math.ceil 得到不小于某数的最大整数

Math.flfloor 得到不大于某数的最大整数

Math.IEEEremainder 求余

Math.max 求两数中最大

Math.min 求两数中最小

Math.sqrt 求开方

Math.pow 求某数的任意次方, 抛出ArithmeticException处理溢出异常

Math.exp 求e的任意次方

Math.log10 以10为底的对数

Math.log 自然对数

Math.rint 求距离某数最近的整数（可能比某数大，也可能比它小）

Math.round 同上，返回int型或者long型（上一个函数返回double型）

Math.random 返回0，1之间的一个随机数

public static void main(String[] args) { 

/**

*Math.sqrt()//计算平方根 

*Math.cbrt()//计算立方根 

*Math.pow(a, b)//计算a的b次方 

*Math.max( , );//计算最大值 

*Math.min( , );//计算最小值 

*/ 

System.out.println(Math.sqrt(16)); //4.0 

System.out.println(Math.cbrt(8)); //2.0 

System.out.println(Math.pow(3,2)); //9.0 

System.out.println(Math.max(2.3,4.5));//4.5 

System.out.println(Math.min(2.3,4.5));//2.3 

/**

\* abs求绝对值 

*/ 

System.out.println(Math.abs(-10.4)); //10.4 

System.out.println(Math.abs(10.1)); //10.1 

/**

\* ceil天花板的意思，就是返回大的值 

*/ 

System.out.println(Math.ceil(-10.1)); //-10.0 

System.out.println(Math.ceil(10.7)); //11.0 

System.out.println(Math.ceil(-0.7)); //-0.0 

System.out.println(Math.ceil(0.0)); //0.0 

System.out.println(Math.ceil(-0.0)); //-0.0 

System.out.println(Math.ceil(-1.7)); //-1.0 

/**

\* floor地板的意思，就是返回小的值 

*/ 

System.out.println(Math.floor(-10.1)); //-11.0 

System.out.println(Math.floor(10.7)); //10.0 

System.out.println(Math.floor(-0.7)); //-1.0 

System.out.println(Math.floor(0.0)); //0.0 

System.out.println(Math.floor(-0.0)); //-0.0 

/**

\* random 取得一个大于或者等于0.0小于不等于1.0的随机数 [0,1) 

*/ 

System.out.println(Math.random()); //小于1大于0的double类型的数 

System.out.println(Math.random()+1);//大于1小于2的double类型的数 

/**

\* rint 四舍五入，返回double值 

\* 注意.5的时候会取偶数 异常的尴尬=。= 

*/ 

System.out.println(Math.rint(10.1)); //10.0 

System.out.println(Math.rint(10.7)); //11.0 

System.out.println(Math.rint(11.5)); //12.0 

System.out.println(Math.rint(10.5)); //10.0 

System.out.println(Math.rint(10.51)); //11.0 

System.out.println(Math.rint(-10.5)); //-10.0 

System.out.println(Math.rint(-11.5)); //-12.0 

System.out.println(Math.rint(-10.51)); //-11.0 

System.out.println(Math.rint(-10.6)); //-11.0 

System.out.println(Math.rint(-10.2)); //-10.0 

/**

\* round 四舍五入，float时返回int值，double时返回long值 

*/ 

System.out.println(Math.round(10.1)); //10 

System.out.println(Math.round(10.7)); //11 

System.out.println(Math.round(10.5)); //11 

System.out.println(Math.round(10.51)); //11 

System.out.println(Math.round(-10.5)); //-10 

System.out.println(Math.round(-10.51)); //-11 

System.out.println(Math.round(-10.6)); //-11 

System.out.println(Math.round(-10.2)); //-10 

} 
```


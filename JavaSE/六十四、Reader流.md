# 六十四、Reader流

Reader ： 和InputStream一模一样，唯一的区别就在于读的数据单位不同

继承自Reader的流都是用于向程序中输入数据，且数据的单位为字符（16bit）

16位：一个字符也就是两个字节，使用Reader流读取数据时都是两个字节两个字节往外读的，为什么还

要有这两种两个字节的读取方式呢? 因为有些字符是占2个字节的，如我们的中文字符在Java里面就是占

两个字节的。如果采用一个字节一个字节往外读的方式，那么读出来的就是半个汉字，这样子Java就没

有办法正确的显示中文字符的，所以有必要存在这种流，一个字符一个字符地往外读。

### 6.1.Reader的基本方法

```java
//读取一个字节并以整数的形式返回（0~255） 

//如果返回-1就说明已经到了输入流的末尾 

int read() throws IOException 

//读取一系列字节并存储到一个数组buffer 

//返回实际读取的字节数，如果读取前已到输入流的末尾，则返回-1 

int read(byte[] buffer) throws IOException 

//读取length个字节 

//并存储到一个字节数组buffer，从length位置开始 

//返回实际读取的字节数，如果读取前以到输入流的末尾返回-1. 

int read(byte[] buffer,int offset,int length) throws IOException 

//关闭流释放内存资源 

void close() throws IOException 

//跳过n个字节不读，返回实际跳过的字节数 

long skip(long n) throws IOException
```


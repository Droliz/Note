## 简介
java中IO流主要对于数据操作，将数据分为了：

-   输入流：读数据；
-   输出流：写数据；  


按数据类型分为:
-   字节流: ：
    字节输入流;字节输出流
-   字符流：
    字符输入流;字符输出流

## 字节流

首先字节流是万能的，可以对==任意类型文件进行读写==

主要分为字节输入流和输出流：
InputStream为所有输入字节流的超类，已知的子类都以该类为后缀；(将已经有的文件数据读取出来)
OutputStream为所有输出字节流的超类，已知的子类都以该类为后缀；(主要用于将读取的数据输出到文件)

### 字节输出流

`OutputStream`属于抽象基类，主要是通过子类的形式进行写数据。
`FileOutStream`类属于`OutputStream`类的子类，通过文件输出流用于将数据写入File。

**其构造方法**：`FileOutputStream(String name, boolean append)`:创建文件输出流以指定的名称写入文件，第二个参数表示是否为追加写入（默认false）

-   创建字节输出流对象(这里创建字节输出流对象，是系统进行了创建了文件，并由字符输出流去指向该文件，在调用字节输出流对象在文件下进行写数据)。
-   调用字节输出流对象的写数据方法。
-   **释放资源(关闭此文件输出流释放与此流相关联的任何系统资源)** 。

|方法|描述|
|:--|:--|
|`write(int b)`|将指定的字节写入文件输出流，一次一个字节|
|`write(byte[] b)`|将字节数组中的字节写入，一次一个字节数组元素|
|`write(byte[] b, int off, int len)`|从指定的字节数组写入len字节，从偏移off开始输出到此输出流|

换行算为一个字节，且在windows中换行可用`\n\r` linux中只能`\n` mac 只能 `\r`

```java
public static void file() throws IOException {
	File file = new File("E:\\test.txt");   // 会自动创建文件，但是不会自动创建目录  
	FileOutputStream fos = new FileOutputStream(file);  // 没有指定追加，会覆盖  
	fos.write(97);   // 'a'  
	byte[] b = {97, 98, 99, 100, 101, 102};  
	fos.write(b, 2, 1);   // 从 2 开始往后 1 位  
	fos.close();
}
```

### 字节输入流

字节流读数据主要通过FileInputStream类进行，继承于InputStream类，(输入流，主要用于读取数据)

|方法|描述|
|:--|:--|
|`read(int b)`|读取一个字节|
|`read(byte[] b)`|读取最多b.length字节到缓冲区数组 b中|
|`read(byte[] b, int off, int len)`|读取len字节数据缓冲区数组 b中|
|`skip(long n)`|跳过和丢弃 n 个字节，返回long|

当读取的数据末尾后，在读取则返回-1；其中read()返回值为读取的字节的ascall值。read(byte[] b)返回值为每次读取的字节数

```java
File file = new File("E:\\test.txt");  
FileInputStream fis = new FileInputStream(file);  
byte[] b = new byte[1024];  // 1024 及整数倍  
int n;  
// 输出  
while ((n = fis.read(b)) != -1) {  
    System.out.print(new String(b, 0, n));  
}  
fis.close();
```

## 字符流常用方法

BufferOutputStream:
该类实现缓冲输出流。通过设置这样的输出流，应用程序可以向底层输出流写入字节，而不必每个字节就调用底层系统。
BufferInputStream:
创建BufferInputStream将创建一个内部缓冲区数组，当从流中读取或者跳过字节时，内部缓冲区将根据需要从包含的输入流中重新填充，一次很多字节。
构造方法
字节缓冲输出流
BufferOutputStream(OutputStream out)；
字节缓冲输入流
BufferInputStream(IntputStream In)；
其原因(因为字节缓冲流只是提供一个缓冲区域来提高效率，然而其真正的操作是需要在字节流区实现的，所以所需要参数为字节流)。

**读取**
   
```java
// 读取单个字符
int read()
// 将字符读入数组
int read(char[] cbuf)
// 将字符读入数组的某一部分
abstract int read(char[] cbuf, int off, int len)
// 跳过字符
long skip(long n)

// 关闭该流并释放与之关联的所有资源
abstract void close()
```

**写入**

```java
// 写入字符数组
void write(char[] cbuf)
// 写入字符数组的某一部分
abstract void write(char[] cbuf, int off, int len)
// 写入单个字符
void write(int c)
// 写入字符串
void write(String str)
// 写入字符串的某一部分
void write(String str, int off, int len)

// 将指定字符添加到此 writer
Writer append(char c)
// 将指定字符序列添加到此 writer
Writer append(CharSequence csq)
// 将指定字符序列的子序列添加到此 writer.Appendable
Writer append(CharSequence csq, int start, int end)

// 关闭此流，但要先刷新它
abstract void close()
// 刷新该流的缓冲
abstract void flush()
```

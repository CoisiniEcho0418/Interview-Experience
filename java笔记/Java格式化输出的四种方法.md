# Java格式化输出的四种方法

------

### 一、System.out.printf()

Java SE5推出了C语言printf()风格的格式化输出功能。

```java
String str="Java";
double pi=3.14;
int i=100;
//"%"表示进行格式化输出，其后是格式的定义
System.out.printf("%f\n",pi);//"f"表示格式化输出浮点数
System.out.printf("%d\n",i);//"d"表示格式化输出十进制整数
System.out.printf("%o\n",i);//"o"表示格式化输出八进制整数
System.out.printf("%x\n",i);//"x"表示格式化输出十六进制整数
System.out.printf("%s\n",str);//"s"表示格式化输出字符串
System.out.printf("一个字符串：%s，一个浮点数：%f，一个整数：%d",str,pi,i);//可以一次输出多个变量，注意顺序即可
```



### 二、System.out.format()

Java SE5引入的format()方法模仿C的printf()方法，可用于PrintStream或者PrintWriter对象，包括System.out对象。用法基本上和System.out.printf()类似。

```java
String str="Java";
double pi=3.14;
int i=100;
//"%"表示进行格式化输出，其后是格式的定义
System.out.format("%f\n",pi);//"f"表示格式化输出浮点数
System.out.format("%d\n",i);//"d"表示格式化输出十进制整数
System.out.format("%o\n",i);//"o"表示格式化输出八进制整数
System.out.format("%x\n",i);//"x"表示格式化输出十六进制整数
System.out.format("%s\n",str);//"s"表示格式化输出字符串
System.out.format("一个字符串：%s，一个浮点数：%f，一个整数：%d",str,pi,i);//可以一次输出多个变量，注意顺序即可
```



### 三、Fomatter类

Java中所有的格式化功能都由java.util.Formatter类处理。当你创建一个Formatter对象时 ，需要向其构造器传递一些信息，告诉它最终的结果将向哪里输出。

```java
import java.util.Formatter;//使用Formatter类时需要导入java.util.Formatter

Formatter f=new Formatter(System.out);//创建一个Formatter对象，指定输出为System.out
String str="Java";
double pi=3.14;
int i=100;
//"%"表示进行格式化输出，其后是格式的定义
f.format("%f\n",pi);//"f"表示格式化输出浮点数
f.format("%d\n",i);//"d"表示格式化输出十进制整数
f.format("%o\n",i);//"o"表示格式化输出八进制整数
f.format("%x\n",i);//"x"表示格式化输出十六进制整数
f.format("%s\n",str);//"s"表示格式化输出字符串
f.format("一个字符串：%s，一个浮点数：%f，一个整数：%d",str,pi,i);//可以一次输出多个变量，注意顺序即可
```



### 四、String.format()

String.format()是一个static方法，接收与Formatter.format()一样的参数，其返回值：String对象，适用于一次输出。

```java
String str="Java";
double pi=3.14;
int i=100;
//"%"表示进行格式化输出，其后是格式的定义
System.out.println(String.format("%f",pi));//"f"表示格式化输出浮点数
System.out.println(String.format("%d",i));//"d"表示格式化输出十进制整数
System.out.println(String.format("%o",i));//"o"表示格式化输出八进制整数
System.out.println(String.format("%x",i));//"x"表示格式化输出十六进制整数
System.out.println(String.format("%s",str));//"s"表示格式化输出字符串
```


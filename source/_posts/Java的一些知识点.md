---
title: Java的一些知识点
categories: Java
date: 2019-11-23 15:55:34
tags:
---

## 第一章

Java开发的时候jre和jdk是两个安装，不要混淆。

使用CMD编译的时候不要放在c盘，可能没有权限。

如果使用的jre和jdk版本不一样会编译出错

## 第二章

java中美元符号$也可以作为标识符

java中常量的声明：final 数据类型 常量名称
比如 final double PI =3.1415926
常量名通常使用大写字母，这样可以清楚的将常量与变量区分开

java基本数据类型
1.整数类型（byte short int long）浮点类型（float double）
2.字符类型
3.布尔类型
byte为1字节8位，取值为128
由于long型的取值范围比int型大，且属于高级的数据类型，所以在赋值的时候要在后面加L或l
比如 long number =1234567L;

浮点类型的数默认是double，所以声明float要在后面加上f

布尔型判断程序：

``` java
import java.util.Scanner;
public class LoginService{
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入6位数字密码：");
        int password = sc.nextInt();
        boolean result = (password==924867);
        System.out.println("用户密码是否正确："+ result);
        sc.close();
    }
}
```

位移运算符 << >> 不会改变数字的正负。
\>>> 无符号右移只会产生整数。

类型转换的时候高精度向低精度转换的时候一定要显式转换

java中变量一定要初始化吗？
java中的变量包括成员变量和局部变量。其中，成员变量声明之后不论是否对其初始化，java虚拟机都会将其初始化默认值。局部变量声明之后，java虚拟机不会将其初始化为默认值，此时开发人员必须对其进行显式的初始化。

## 第三章

print和println的区别：print输出字符时不会换行，而println输出字符是会自动换行。

java中if条件只能是布尔型变量或者是布尔表达式。不能用整数或是字符，因为不能自动将整数转化为布尔型变量。

在switch多分支语句中，case后面的常量表达式的值可以是int、short、char、byte、String以及enum

foreach语句是for语句的简化版本。
for(循环变量x：遍历对象obj)
{
	引用了x的java语句
}
利用foreach遍历数组成员

```java
public class foreach
{
    public static void main(String[] args) {
        int arr[]={7,10,1};
        System.out.println("一维数组中的元素分别为：");
        for (int x : arr) {
            System.out.println(x);
        }
    }
}
```

java中如果想要跳出外层循环，可以用标签功能。
标签名：循环体｛break 标签名；｝
continue也是一样

## 第四章

创建一维数组的方法int arr[]  int[] arr

声明数组后还不能访问任何元素，还需要分配内存空间。
分配方法：`arr=new int[5]`

获取数组长度方法：`arr.length`

使用fill方法将空数组填满数值
方法`Arrays.fill(int[] a , int value)`

## 第五章

利用构造方法实例化字符串：String a = new String("string");

利用字符数组实例化：char[] charArray = {'t','I','m','e'};
String a = new  String(charArray);

提取字符数组中的一部分创建字符串对象
Char[] charArray = {};
String a = new String(charArray,3,2);
表示从字符数组索引3的位置提取两个元素构成字符串

使用加号将字符串进行连接时，只要加号一边是字符串，就会自动调用toSting()将另外一边转换为字符串进行连接。

长度：`str.length()`
指定位置字符：`str.charAt(index)`
获取子字符串索引位置：`a.indexOf(substr)`
如果没有会返回-1

判断字符串首尾内容：`str.startsWith(prefix)  str.endsWith(suffix)`

获取字符数组：`str.toCharArray();`
判断子字符串是否存在：`str.contains(string);`

截取字符串：`str.substring( beginIndex,endIndex);`
其中起始索引包括，结束索引不包括。

```java
import javax.print.DocFlavor.STRING;
public class IDcard{
    public static void main(String[] args) {
        String idNum= "123456198002157890";
        String year = idNum.substring(6,10);
        String month = idNum.substring(10, 12);
        String date = idNum.substring(12, 14);
        System.out.println("该身份证显示的出生日期是：");
        System.out.println(year+"年"+month+"月"+date+"日");
    }
}
```

字符串替换：`str.replace(oldstr,newstr);`
返回一个字符串，如果没有找到需要被替换的字符串就会返回原字符串。
如果要替换的字符串出现多次，会全部替换。

字符串分割：`str.split(regex)regex：分隔符`
根据指定的分隔符对字符串进行拆分

大小写转换
`str.toLowerCase()`
`str.toUpperCase()`

去除空白内容：`str.trim()`
可以将字符串首尾处的空白内容都删除。

比较字符串是否相等：
`a.equals(str);`
当且仅当进行比较的字符串不为NULL且被比较的字符串内容相同，结果才为true；

Java中String一旦赋值将无法修改，每次对String值的修改都是返回新的String。

可变字符串StringBuffer类
StringBuffer类创建的对象是可以修改的。

创建一个StringBuffer类必须使用关键字new，不能像String类里那样直接引用字符串常量。
`StringBuffer sbf = new StringBuffer();`

Append方法
`Sbf.append(obj);`可以将参数转化为字符串，然后追加到此序列中。

setCharAt方法：`sbf.setCharAt(index,ch);`
将给定索引处的字符修改为ch

insert方法：`insert(offset,str);`
将字符串插入到指定的索引处。

delete方法：`sbf.delete(start,end)`

StringBuffer类中也有很多类似于String类的方法。

```java
public class StringBufferTest{
    public static void main(String[] args) {
        StringBuffer sbf = new StringBuffer("ABCDEFG");
        System.out.println("sbf的原值是"+sbf);
        int length = sbf.length();
        System.out.println("sbf的长度为"+length);
        char chr = sbf.charAt(5);
        System.out.println("索引为5的字符为："+chr);
        int  index = sbf.indexOf("DEF");
        System.out.println("DEF的位置为:"+index);
        String substr = sbf.substring(0,2);
        System.out.println("索引0到2之间的字符串："+substr);
        StringBuffer tmp = sbf.replace(2,5,"1234");
        System.out.println("替换后的字符串："+tmp);
    }
}
```

StringBuffer与String的不同之处
StringBuffer每次都是操作自身对象，而不是生成新的对象，其所占的空间会随着字符内容增加而扩充，做大量的修改操作时，不会因生成大量匿名对象而影响系统性能。 

## 第六章

定义类的时候加public表示全局类，该类可以import到任何类内。不加public默认为保留类，只能被同一个包内的其他类引用

成员方法中的不定长参数：
声明方法时，如果有若干个相同类型的参数，可以定义为不定长参数，该类型的参数声明如下：权限修饰符 返回值类型 方法名（参数类型… 参数名）

构造方法 

```java
class Book{
    public Book(){
        
    }
}
```

public要写，不然在类外无法初始化新的对象。

无参数构造方法: 

```java
public Make(){
    this(1);
}
```

static关键字
当一个类中两个不同的方法对同一个变量进行操作的时候，共享的变量用static修饰，那么该变量就是静态变量。

静态方法
如果想要使用一个类中的成员方法，但是不想或者不能创建该类的对象时，就可以用静态方法。

静态代码块

```java
public class StaticTest{
    static{
       //此处编辑执行语句     
    }
}
```

代码块的执行顺序：

```java
public class StaticTest{
    static String name;
    static{
        System.out.println(name+"静态代码块");
    }
    {
        System.out.println(name+"非静态代码块");
    }
    public StaticTest(String a)
    {
        name = a;
        System.out.println(name+"构造方法");
    }
    public void method(){
        System.out.println(name+"成员方法");
    }
    public static void main(String[] args) {
        StaticTest s1;
        StaticTest s2 = new StaticTest("s2");
        StaticTest s3 = new StaticTest("s3");
        s3.method();
    }
}
```

静态代码块由始至终只运行了一次
非静态代码块，每次创建对象的时候，会在构造方法之前运行。所以读取成员变量name时，只能获取到String类型的默认值“null”
构造方法只有在使用new创建对象的时候才会运行。
成员方法只有在使用对象调用的时候才会运行。因为name是static修饰的静态成员变量，在创建s2对象时将字符串“s2”赋给了name，所以创建s3对象时，重新调用了类的非静态代码块，此时name的值还没有被s3对象改变，于是就会输出“s2非静态代码块”

第七章
类的继承：child extends parents
注意：java仅支持单继承，即一个类只能有一个父类。

方法的重写：重写方法实质上是在子类中将父类方法覆盖。
重写时修改方法的修饰权限只能从小到大，类似protected只能到public不能到private；
super关键字：如果重写后还想调用父类方法，可以用super.父类方法来调用。

```java
class Computer{
    String sayHello(){
        return "欢迎使用";
    }
}
public class Pad extends Computer{
    String  sayHello(){
        return super.sayHello()+"iPad";
    }
    public static void main(String[] args) {
        Computer pc = new Computer();
        System.out.println(pc.sayHello());
        Pad pad = new Pad();
        System.out.println(pad.sayHello());
    }
} 
```

The public type must be defined in its own file
翻译：公共类必须被定义在它自己的文件中
产生错误的原因：
1.定义Java类名同文件名不一样
2.一个文件中有多个类，只有与文件名一致的类名，才能声明为：public
内部类不能声明为：public
一个文件中只能有一个public类

Object类：
在java中所有的类都直接或间接继承了java.lang.Object类
Object类中主要包括clone()、finalize()、equals()、toString()等方法。由于所有的类都是Object的子类，因此这些函数都可以重写。

getClass()方法：
返回某个对象执行时的Class实例。getClass().gerName();获取类的名称。

toString()返回某个对象的字符串表示形式。

equals()方法比较的是两个对象的引用地址是否相等。比如两个字符串是相等的但是两个对象就是不相等的。

在java中函数参数出现顺序不一样也可以构成重载关系。

向上转型：子类当作父类用，但是就不能用子类独有的方法。
F p = new s()；
p.methodsOfS()就不可以调用。

向下转型一般不要使用，容易出现逻辑错误。





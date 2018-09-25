---
title: java学习笔记
author: 0x17
date: 2018-07-24 17:04:53
tags:
---

### 包装类

首先了解java中的八种基本类型：
```
byte(1byte),short(2bytes),int(4bytes),long(8bytes)
float(4bytes),double(8bytes)
char(2bytes)
boolean(暂未确定，依赖Java虚拟机具体实现)
```
[参考](https://www.jianshu.com/p/2f663dc820d0))

#### 什么是包装类

为了将基本类型封装面向对象，设计类时将八种基本类型设计了对应的类，这样的类称为包装类，位于java.lang包中。

#### 包装类的作用

1.有时候方法需要传递一个Object参数，这是就可以使用包装类。

2.集合不允许存放基本类型数据。

3.包装类中封装了一些方法和常量，比如类型最大值，类型转换方法。

4.String可以和基本类型互相转换。

#### 使用

自动装箱：基本类型转包装类

自动拆箱：包装类转基本类型

#### 注意

包装类中除了Float与Double，都有缓存值，如果在缓存值的范围内，直接引用缓存值（-128-127）。[参考](https://blog.csdn.net/qq_29119581/article/details/78327759)

### Singleton

#### 为什么要使用单例模式

1.控制资源的使用，通过线程同步来控制资源的并发访问；

2.控制实例产生的数量，达到节约资源的目的。

3.作为通信媒介使用，也就是数据共享，它可以在不建立直接关联的条件下，让多个不相关的两个线程或者进程之间实现通信。

[参考](https://www.cnblogs.com/andy-zhou/p/5363585.html)

#### 饿汉模式

```
public class Singleton{
    private static Singleton singleton = new Singleton();
    private Singleton(){}
    public static Singleton getSingleton(){
        return singleton;
    }
}
```
特点：简单安全，但是无法达到延迟加载

#### 懒汉模式

```
public class Singleton{
    private static Singleton singleton = null;
    private Singleton(){}
    public static Singleton getSingleton(){
        if(singleton == null) singleton = new Singleton();
        return singleton;
    }
}
```
特点：可以延迟加载但是线程不安全，会导致创建多个实例

[其他优化模式](https://www.cnblogs.com/andy-zhou/p/5363585.html)

### java中的封装、继承、多态与组合

封装：将类中的属性定义为私有的，然后对象通过类中暴露公有的方法来对数据进行操作，这样我们通过自定义公有方法来控制数据的方法方式。

继承：子类继承超类的属性和方法，还可以对超类进行拓展。

多态：比如说一个接口可以有不同的实现，一个标准有不同的实现。

组合：所以总结来说，“is-a ”（是一个）的关系是用继承来表达的，而“has-a"（有一个）的关系则是用组合来表达的。比如说汽车属于一种交通工具，这是一种继承关系，而汽车拥有发动机，门窗等零部件，这是组合的关系。

### java抽象类和接口

只要类中有抽象方法那么这个类就是抽象类，或者直接被修饰为abstract。

接口算是一个特殊的抽象类，其中所有的变量都为静态常量，所有方法都为静态方法，且没有静态方法。

一个类如果继承抽象类，那么必须得实现抽象类中所有的抽象方法，否则它也是一个抽象类。

一个类可以实现多个接口。

### Java内部类，内部私有类的作用

### java合集

#### 搞清楚其中类和接口的关系关系

![Please Wait](2018_9_6.gif)

#### Iterator与Iterable

Iterator与Iterable都是接口，从字面上可以分别理解为迭代器和可迭代的。

Iterator中有hasNext(), next(), remove(), forEachRemaining()等方法。

Iterable中有一个iterator方法，用来返回一个iterator，也就是返回一个迭代器

### comparable与comparator

### 序列化与反序列化

序列化：把对象转换为字节序列的过程称为对象的序列化

反序列化：把字节序列恢复为对象的过程称为对象的反序列化

作用：

1.把对象的字节序列永久地保存到硬盘上，通常存放在一个文件中　
   
如：在很多应用中，需要对某些对象进行序列化，让它们离开内存空间，入住物理硬盘，以便长期保存。比如最常见的是Web服务器中的Session对象，当有 10万用户并发访问，就有可能出现10万个Session对象，内存可能吃不消，于是Web容器就会把一些seesion先序列化到硬盘中，等要用了，再把保存在硬盘中的对象还原到内存中。

2.在网络上传送对象的字节序列
   
如：当两个进程在进行远程通信时，彼此可以发送各种类型的数据。无论是何种类型的数据，都会以二进制序列的形式在网络上传送。发送方需要把这个Java对象转换为字节序列，才能在网络上传送；接收方则需要把字节序列再恢复为Java对象。

### 面向接口编程

### 异常处理

### io中各种类的关系与作用

### 多线程编程

### tomcat与apache与nginx的区别

Apache HTTP服务器和Nginx服务器都是http服务器，用来存放静态资源，比如说image,css,html等文件，有一些编程语言中的类库也都可以实现一个简单的http服务器，比如说java或者node。Tomcat服务器除了以上功能还可以发送动态资源，比如说通过Java Server Pages技术可以添加Java语句，jsp本质上就是一个servlet，Tomcat是一个能够运行servlet的容器，管理servlet的生命周期、将url映射到servlet进行处理等。

Nginx除此之外还是一款优秀的反向代理服务器

#### 反向代理与负载均衡

[反向代理](https://www.zhihu.com/question/24723688/answer/128105528)：

**正向代理**代理的是客户端，隐藏了客户端的信息

**反向代理**代理的是服务器，隐藏了服务器的信息

[负载均衡](https://zhuanlan.zhihu.com/p/32841479):

将工作分布到多个服务器（能处理相同的请求）来提高应用的性能和可靠性，代理服务器能通过高效的算法将请求转发到性能良好的服务器中进行处理。

### java中的内存管理

java中的内存区域大致可分为CPU寄存器、栈、堆、静态域、常量池等。

#### 字符串的两种创建方式

```
//第一种创建方式
String s1 = "hello";
String s2 = "hello";
String s3 = "world";
//第二种创建方式
String str1 = new String("hello");
String str2 = new String("hello");

System.out.println("s1==s2:"+(s1==s2));//true
System.out.println("s1==str1:"+(s1==str1));//false
System.out.println("str1==str2:"+(str1==str2));//false

String s4 = "hello"+" "+"world";
String s5 = s2+" "+s3;
String s6 = "hello world";
System.out.println("s4==s6:"+(s4==s6));//ture
System.out.println("s5==s6:"+(s5==s6));//false
```
第一种创建字符串的时候，会在常量池中查找是否有该对象，如果有，直接指向该内存地址，如果没有，先在常量池中创建，然后再指向其地址。这个过程在编译期就执行好了。

第二种创建字符串，使用String的构造方法，在堆中创建String对象，然后String中的value在常量池中查找是否有该常量，该过程在运行期执行。

```
//String的构造方法
public String(String original) {
        this.value = original.value;
        this.hash = original.hash;
    }
```

#### 垃圾回收机制

### java注解

#### 元注解

元注解也是注解，它是用来给自定义注解标识的信息的。

### String StringBuilder StringBuffer

String:适用于少量的字符串操作。

StringBuilder:适用于单线程下在字符缓冲区进行大量操作的情况。

StringBuffer:适用于多线程下在字符缓冲区进行大量操作的情况（线程安全）。

### Java泛型（generic）

Java泛型可以参数化类型，泛型存在于编译时期，如果泛型类型是安全的，就将泛型转换为原始类型，所以泛型的优点是可以在编译时检查类型错误，提高了代码的可靠性和可读性。

**泛型类和泛型接口**：可以为类定义泛型，那么在创建对象或者是声明一个引用变量的时候就要指明其类型。

**泛型方法**：可将类型作为参数传给方法，在方法前需要形式泛型类型，调用时再实例化泛型

**泛型类**，是在实例化类的时候指明泛型的具体类型；泛型方法，是在调用方法的时候指明泛型的具体类型（泛型方法调用形式object.<T>function(param),或者编译器通过传递的参数自动发现类型） 。

**泛型方法与可变参数**

```
public <N> void printMsg(N ...args){
    for(N n : args){
        System.out.println("typeof n is" + n.getClass());
    }
}

printMsg("111",222,"aaaa","2323.4",55.55);

输出
typeof n isclass java.lang.String
typeof n isclass java.lang.Integer
typeof n isclass java.lang.String
typeof n isclass java.lang.String
typeof n isclass java.lang.Double
```

**通配泛型与受限泛型**

非受限泛型:<T>  <<==>>  <T extends Object>
受限泛型:<E extends T> 为T类型的子类
非受限通配泛型:<?>  <<==>>  <? extends Object>
受限通配泛型:<? extends T> 为T或为T的子类型
下限通配泛型:<? super T> 为T或为T的父类型

```
A<Number>
B<Integer>
虽然Integer是Number的子类型，但是B<Integer>不是A<Number>的子类型，为了这个问题使用通配泛型
A<? extends Number>
或者A<?>
举个例子
public class Generic {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        test1(list);
        test2(list);
        test3(list);
        test4(list);
        //test5(list);
    }
    public static void test1(ArrayList<?> list){
        for (Object object: list){
            System.out.println(object+" type"+object.getClass());
        }
    }
    //？为通配符，所以在这里可以不用声明方法为泛型方法，如果使用泛型T的话，就要声明为泛型方法，但是受限又冲突了
    public static void test2(ArrayList<? extends Number> list){
        for (Object object: list){
            System.out.println(object+" type"+object.getClass());
        }
    }
    public static <T> void test3(ArrayList<T> list){
        for (Object object: list){
            System.out.println(object+" type"+object.getClass());
        }
    }
    public static <T extends Number> void test4(ArrayList<T> list){
        for (Object object: list){
            System.out.println(object+" type"+object.getClass());
        }
    }
    //这是错误的写法，编译不通过，(unexpected bound)因为已经给方法声明为泛型了，然后在传参的时候就不能让泛型受限了
    /*public static <T> void test5(ArrayList<T extends Number> list){
        for (Object object: list){
            System.out.println(object+" type"+object.getClass());
        }
    }*/
}
```

**注意事项**:

泛型类的构造方法不用加<T>

泛型必须是引用类型，所以在传递基本类型的时候都会自动装箱。

如果泛型类中的静态方法使用泛型，那么一定要为该静态方法形式泛型类型（一定为泛型静态方法）。
（有些不大明白）

异常类不能是泛型，因为异常类是泛型的话，那么catch中的也是泛型，但这是运行时才知道的。



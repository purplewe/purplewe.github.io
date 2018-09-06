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

包装类中除了Float与Double，都有缓存值，如果在缓存值的范围内，直接引用缓存值。[参考](https://blog.csdn.net/qq_29119581/article/details/78327759)

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

### java中的继承多态与封装

### java抽象类和接口

### java集合

#### 搞清楚继承关系

#### Iterator与Iterable

### 序列化与反序列化

### 面向接口编程

### 抽象类与接口

### 异常处理

### io中各种类的关系与作用

### 多线程编程

### tomcat与apache与nginx的区别

apache服务器和nginx服务器都是

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

### java注解详解

### String StringBuilder StringBuffer




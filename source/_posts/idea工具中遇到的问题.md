---
title: idea工具中遇到的问题
date: 2020-3-6 22:51:03
tags:
---
没打断点，输出正常


![Please Wait](1.png)


打断点后，输出异常，多次输出

![Please Wait](2.png)



idea在调试的过程中会调用代理类的toString方法，这一点可以通过打印method.getName查看。那么如何确定是调用代理类的方法呢，为什么idea调试过程会调用这个方法呢？现在我还不清楚，但是思路应该是：

1.需要了解InvocationHandler中invoke方法是什么时候被调用的。

需要反编译proxy生成的代理类，查看它调用的位置。

2.为什么接口类型的方法变量调用toString() equals()等方法。

> If an interface has no direct superinterfaces, then the interface implicitly declares a public abstract member method m with signature s, return type r, and throwsclause t corresponding to each public instance method m with signature s, return type r, and throws clause t declared in Object, unless a method with the same signature, same return type, and a compatible throws clause is explicitly declared by the interface.
> 
一个顶级接口内部会隐式声明Object中相应的public的方法，所以接口类型的变量是可以调用toString()等方法的，在编译期不会报错。但是接口类型的变量最终还是要指向一个具体的对象（多态--->动态绑定），那我接口里面声明的这些方法又有啥用呢？

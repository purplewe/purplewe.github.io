---
title: 每天一点es6
author: 'peng'
date: 2019-03-28
---

### 块级作用域

#### 为什么需要块级作用域

es5中只有块级作用域和函数作用域，这会产生某些不合理的场景。

```
场景一
var a = 'test'

function f(){
    console.log(a);
    if(true){
        var a = 'test1';
    }
}

f() // undefined
本来调用f()希望得到的结果是test,但是由于函数f内的a变量提升到该函数的顶部，导致输出的a为undefined。

场景二
function f(){
    for(var i = 0; i < 10; i++){}
}
i // 10
循环中计数的循环变量泄露为上一层作用域的变量。
```

#### es6中的块级作用域

es6中的let的出现实际为JavaScript增加了块级作用域

```
function f1(){
    let n = 5;
    if(true){
        let n = 10;
    }
    console.log(n) // 5
}
这表明外层块级作用域不受内层块级作用域的影响。

(function(){
...
}())

{
...
}
块级作用域的出现实际上使得自执行函数不再必要了。
```

### let命令

let命令：let命令用来声明的变量，其声明的变量只在其所在的代码块中有效。

#### 用法

```
es5中的作用域只有函数作用域和全局作用域，在函数体中使用var定义的变量，会被提到函数开始处进行定义，作用域为整个函数。
var a = [];
for (var i = 0; i < 10; i++) {
    a[i] = function () {
        console.log(i);
    };
  }
a[6](); // 10
i; // 10
因为for循环并不是一个函数体，所以for循环中定义的变量的作用域是其所在的函数体，所以这里的i的输出是10，而a[6]()的输出为10是因为i一直保存的是值引用，所以显示10.

var a = [];
for (let i = 0; i < 10; i++) {
     a[i] = function () {
        console.log(i);
     };
  }
a[6](); // 6
因为let声明的变量只在块级作用域中生效，每一个循环都会产生一个新的作用域。

for(let i = 0; i < 3; i++){
    let i = 'abc';
    console.log(i);
}
// abc
// abc
// abc
实际上在这里,整个for循环有两个作用域，设置循环变量的部分为父作用域，循环体那部分为子作用域。因为这个循环执行了三次，父作用域没有受到子作用域的影响。
```

#### 不存在变量提升

变量提升：指的是变量可以在变量声明前使用该变量，但是会输出undefined。

为了纠正这种情况，使用let声明的变量不可以在变量声明前使用，否则会报错。

```
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```

#### 暂时性死区

在代码块内，使用let命令声明变量之前，该变量都是不可用的，该现象称为“暂时性死区”(temporal dead zone)

```
x = 'a';
let x = 'b';
会报错：该变量未定义

typeof x; //报错
let x = 'a';
typeof undeclear_var // undefined
会使得typeof操作符的使用不再安全

var x = x //undefined
let x = x //报错
```

#### 不允许重复声明

在同一块级作用域中不允许重复声明变量。

```
function f(args){
    let args;
} //报错

function f1(args){
    {
    let args;
    }
} //不报错
```

### const命令

const声明的变量指向的地址所保存的数据不可变，一旦声明后就不可以修改，一旦声明就要初始化。

const与let一样，只在声明的块级作用域中有效，并且声明的常量也不可以提升，同样存在暂时性死区。

```
const a = {};
a.property = 1; //不报错
a = {}; //报错 
只要保证a变量指向地址保存的数据不改变就不会报错

function f(obj){
    Object.freeze(obj);
    Object.keys(obj).forEach((key) => {
        if(typeof obj[key] === 'object'){
            f(obj[key]);
        }
    })
}
```

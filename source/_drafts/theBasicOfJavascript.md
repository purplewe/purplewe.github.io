---
title: Javascript基础
author: '0x17'
date: 2019-03-27
---

### 解构运算符、拓展运算符、rest运算符

解构运算符：作用是快速获取对象或数组中的元素。

```
变量名与结构对象属性名不一致
var obj = {
    name: 'Jack',
    sex: 'male',
    age: 18
};
var {name: nickname, sex: gender} = obj;
console.log(nickname + ' is a ' + gender);

对象嵌套数组解构
obj = {
    name: 'chris',
    sex: 'male',
    age: [1, 2, 3]
}

{name, sex, age: [a, b, c]} = obj;
console.log(c);

对象嵌套对象属性名重复时解构
var obj = {
    name: 'chris',
    sex: 'male',
    age: 26,
    son: {
        name: '大熊',
        sex: 'male',
        age: 1
        }
    };
    //赋值解构
var {name: fathername, son: {name, sex, age}} = obj;
console.log(fathername); //chris
console.log(name);

变量值互换


字符串解构
```

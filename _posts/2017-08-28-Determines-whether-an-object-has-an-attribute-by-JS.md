---
title: JS检测对象是否具有某个属性
description: JS检测对象属性
categories: technology
tags: JS
---

> 看书的时候正好碰到这个问题，查了一些资料，对JS检测对象属性有了一个粗浅的认识。

## obj.hasOwnProperty(prop)检测对象本身的属性

非常符合语义，用于检测一个对象本身（**不包括原型链**）是否具有指定名称的属性，返回布尔值。

```javascript
o = new Object();
o.prop = 'exists';
o.hasOwnProperty('prop');             // 返回 true
o.hasOwnProperty('toString');         // 返回 false
o.hasOwnProperty('hasOwnProperty');   // 返回 false
```

## prop **in** obj检测对象本身+原型链的属性

如果要检测一个对象（**包括原型链**）是否具有指定名称的属性，则使用prop **in** object ,仍然返回布尔值。

```javascript
// 数组
var trees = new Array("redwood", "bay", "cedar", "oak", "maple");
0 in trees        // 返回true
3 in trees        // 返回true
6 in trees        // 返回false
"bay" in trees    // 返回false (必须使用索引号,而不是数组元素的值)

"length" in trees // 返回true (length是一个数组属性)

Symbol.iterator in trees // 返回true (数组可迭代，只在ES2015+上有效)


// 内置对象
"PI" in Math          // 返回true

// 自定义对象
var mycar = {make: "Honda", model: "Accord", year: 1998};
"make" in mycar  // 返回true
"model" in mycar // 返回true

"toString" in {}; // 返回true
```
注意：obj.hasOwnProperty(prop) 和 prop **in** obj中的prpo均为字符串或symbol
```javascript
o = new Object();
o.prop = 'exists';
o.hasOwnProperty(prop);    // 返回 false
o.hasOwnProperty('prop');  // 返回 true
prpo in e;                 //false
'prpo' in e;               //true
```

## for (variable in object) {...}遍历对象的可枚举属性(本身+原型链)

for...in 语句用于遍历一个对象的可枚举属性，**只遍历可枚举属性**。例如Array 和 Object 使用内置构造函数所创建的对象都会继承自 Object.prototype 和 String.prototype 的不可枚举属性，例如 String 的 indexOf()  方法或者 Object 的 toString 方法均不可遍历。

for in循环不仅会遍历array对象的下标还会将length等属性都枚举出来，且for...in 循环以任意序迭代一个对象的属性，**for..in 不应该被用来迭代一个下标顺序很重要的 Array(能用，但慎用)**

for...in 循环以任意序迭代一个对象的属性,不要在迭代过程中删除，添加或者修改属性。

若某个对象包含值为undefinded的属性，使用hasOwnProperty或者in检测均返回true,但如果使用delete将某个属性赋值为undefinded,检测将返回false.

for...in结合hasOwnPropetry可遍历对象本身的属性.

非包装类型的对象hasOwnProperty和in只能检测对象创建时就有的属性，添加的属性不能检测
```javascript
var a = [1,6,7];
a[7] = 9;

1 in a;  //true
5 in a;  //false

var b = new Array('asd',123);
3 in b;    //false

b[5] = 'fg';
3 in b;    //true
```

## Object.getOwnPropertyNames(obj)和Object.keys() 

Object.getOwnPropertyNames(obj)
返回一个由指定对象的所有自身属性的属性名（包括不可枚举属性）组成的数组。

Object.keys() 
返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和使用 for...in 循环遍历该对象时返回的顺序一致 （两者的主要区别是 一个 for-in 循环还会枚举其原型链上的属性）

```javascript
var target = myObject;
var enum_only = Object.keys(target);
var indexInEnum = enum_only.indexOf(prpo);
if (indexInEnum == -1) {
    console.log("该对象没有prpo属性")
}
```
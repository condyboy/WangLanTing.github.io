---
title: JS学习（一）基本语法
date: 2019-08-09 14:51:56
tags: JS
categories: JS
---
# JS学习（一）基本语法
一、基本语法
1、定义变量`var`
2、每个语句以`;`结束，语句块用`{...}`扩起。
3、注释使用`//`多行注释使用`/***/`
4、比较运算符中`==`和`===`的区别为，前者比较时会自动转换数据类型再比较，而第二种===，它不会自动转换数据类型，如果数据类型不一致，就返回`false`
5、`NaN`表示not a number，表示无法计算结果。它与其他值都不相等，包括它自己。唯一的判断方法是`isNaN(NaN)`
6、比较浮点数，浮点数在运算过程中会产生误差，因为计算机无法精确表示无限循环小数，当比较浮点数是否相等时，只能计算它们之差的绝对值，是否小于某个阈值：

```
Math.abs(1/3-(1-2/3))<0.0000001;
```
7、`null`表示空值，undefined表示未定义
8、创建数组

```
var arr=[1,2,3,4,'he',null];
var arr=new Array(1,2,3)
```
9、对象：键值对的形式（就像map一样）

```
var a={
    name:'Bob',
    age:20
}
```
10、控制台打印内容
使用`console.log(x)`
11、转义
转义字符`\`，举例换行符和制表符`\n \t`，ASCII字符可以以`\x##`表示十六进制，使用`\u####`表示unicode字符如`\u4e2d`就等同于中文。
12、多行字符串用反引号（1的左边）来表示

```
`
这是
一个
多行字符串
`
```
使用`+`进行字符串连接。
13、
将字符串转为大写字母：

```
str.toUpperCase()
```
将字符串转为小写字母

```
str.toLowerCase()
```
返回字符串首次出现的位置
返回子串subs在串s中首次出现的索引值
```
s.indexOf(subs);
```
返回指定索引的字符串

```
s.substring(1,5) //返回索引1~5的字符串
s.substring(7)  //返回从索引7开始的字符串
```
返回字符串的长度

```
s.length
```
14、数组
数组返回指定元素的索引
```
arr.index(element);
```
数组截取部分元素并返回一个新的数组

```
arr.slice(0,3) //返回从0~3的索引
arr.slice(3)  //返回从3到之后的索引
```
向数组末尾添加和删除数据

```
arr.push(element);
arr.pop()
```
向数组头部添加和删除数据

```
arr.unshift(element1,element2.....)  //向arr头部添加数据
arr.shift();  //删除arr头部的数据
```
数组排序

```
arr.sort()
```
数组翻转

```
arr.reverse()
```
15、从指定的索引位置开始删除若干元素，然后再从该位置添加若干元素。

从索引2开始删除3个元素
```
arr.splice(2,3,'a','b')  //返回删除的元素
```
16、连接数组，并返回连接后新的数组

```
var add=arr.concat(newarr)
```
17、将数组用指定字符连接成字符串返回字符串

```
arr.join('-')
```
3、对象
使用键值对的形式对对象进行描述。
例

```
var xiaoming = {
     name: '小红',
     school: 'No.1 Middle School',
    height: 1.70,
    weight: 65,
    score: null
    'middle-school': 'No.1 Middle School'
};
```
可以使用`对象名.对象属性`来获取对象属性的值，如果属性包含特殊字符，使用`''`括起来，可以通过`对象名['对象属性']`
查看对象是否拥有某一属性使用`in`来查看，但是有一些属性是通过继承得到的，要查看其自身拥有的对象`对象名.hasOwnProperty('属性')`
4、遍历对象或数组
使用for循环，或者for循环的变体`for.....in`进行遍历

```
a=[1,2,3]
for (var i in a){
console.log(i)  //i为遍历的索引
console.log(a[i]) //遍历元素值
}
```

```
var a={"name":1,"age":18}
for (var i in a){
console.log(i)  //i为遍历的键值
console.log(a[i]) //遍历元素值
}
```
5、map和set

```
//初始化map
var m = new Map();
m.set("a",67);   //增加
m.has("a");   //查看是否存在key
m.delete("a"); //根据key删除
m.get("a");  //根据键获取值
```

```
var a=new set();
a.add(b);
a.delete(b);
```
6、`iterable`类型，包含`Array`（有索引）、`Map`（key为索引）、`Set`（无序，无索引）类型，它可以通过`for.....of`来进行遍历，`for...in`和`for.....of`的区别，`for....in`遍历为对象添加属性时也会输出属性名(`for....in`遍历输出属性索引值)，而`for....of`只会输出集合的元素内容(`for....of`遍历输出属性内容)。

```
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x in a) {
    console.log(x); // '0', '1', '2', 'name'
}
```

```
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x of a) {
    console.log(x); // 'A', 'B', 'C'
}
```
使用`forEach()`进行迭代

```
a.forEach(function (element, index, array) {
    // element: 指向当前元素的值
    // index: 指向当前索引
    // array: 指向Array对象本身
    console.log(element + ', index = ' + index);
});
```
可以选择需要的参数进行迭代
```
var a = ['A', 'B', 'C'];
a.forEach(function (element,key) {
    console.log(element);
    console.log(key);
});
//输出：
0
A
1
B
2
C
```
map数组迭代：

```
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    console.log(value);
});
```

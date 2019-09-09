---
title: 7-24日报 自定义排序以及lambda和filter
date: 2019-07-24 11:21:27
tags: python
categories: python
---
# 7-24日报
## sort函数和sorted函数
区别：sort函数只能用于list排序，sorted可以用于任何可迭代的对象排序，sort是在原序列上操作，而sorted将排序后的结果重新生成列表。

```
sorted(list,key,reverse)
```
iterable --  可迭代对象。
key --  主要是用来进行比较的元素，只有一个参数，具体的函数的参数就是取自于可迭代对象中，指定可迭代对象中的一个元素来进行排序。
reverse --  排序规则，reverse = True  降序 ， reverse = False 升序（默认）。
## 自定义排序
引入functools，使用sorted函数

key=functools.cmp_to_key(cmp)      
cmp--  自定义比较的函数，这个具有两个参数，参数的值都是从可迭代对象中取出，此函数必须遵守的规则为，**大于则返回1，小于则返回-1，等于则返回0。**

```
import sys
import functools
def cmp1(x,y):    
    if(x<y):return 1    
    else: return -1
x=[-1,6,3,-7,8,4,5,3]
y=sorted(x,key=functools.cmp_to_key(cmp1))
print(y)
```
## lambda表达式
lambda表达式本质上是匿名函数，python中lambda表达式函数体只有一句返回`lambda x,y:x+y`冒号前为参数，后面为函数返回值
例，将列表的按绝对值大小进行排序：

```
import sys
import functools
x=[-1,6,3,-7,8,4,5,3]
y=sorted(x,key=lambda x:abs(x))
print(y)
```
## 常见python内置函数总结
### filter过滤器函数
例：找出平方根是整数的数字`filter(function, iterable)`前面的参数是过滤规则函数，后面参数是能迭代数据

```
import math
x=[1,3,4,5,6,8,9]
temp=filter(lambda x:math.sqrt(x)%1==0,x)
for x in temp:    
    print(x,end=' ')
```
### enumerate()和zip()
前者取出索引和值，后者可以同时取出多个列表的值

```
x=[1,3,4,5,6,8,9]
y=[1,2,3,4,5,8,9]
#取出索引和值
for k,v in enumerate(x):    
    print(k,v)
#同时取出两个列表的值
for k,v in zip(x,y):
    print(k,v)
```
### 构造dictionary

```
map(fuction,iterable)
```

```
x=[1,3,4,5,6,8,9]
result=map(lambda x:x*2,x)
for i in result:
    print(i)

```
### 查看参数所属类型的所有内置方法

```
print(dir(dict))
```


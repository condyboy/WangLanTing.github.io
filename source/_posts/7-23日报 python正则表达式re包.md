---
title: 7-23日报 python正则表达式re包
date: 2019-07-23 20:36:14
tags: 日报
categories: 学习进度
---
# 7-23日报
## 正则表达式整理
python正则表达式，引入re包

```
import re
```
re.match 尝试从字符串的**起始**位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。
### re.match函数

```
re.match(pattern, string, flags=0)
```
pattern是正则表达式，string是要匹配的字串，flags是可选标志
re.I	使匹配对大小写不敏感
re.M	多行匹配，影响 ^ 和 $
re.S	使 . 匹配包括换行在内的所有字符
### re.search函数
 re.search扫描整个字符串并返回**第一个**成功的匹配。

```
re.search(pattern, string, flags=0)
```
同上
re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。
### 检索和替换

```
re.sub(pattern, repl, string, count=0, flags=0)
```
pattern : 正则中的模式字符串。
repl : 替换的字符串，也可为一个函数。
string : 要被查找替换的原始字符串。
count : 模式匹配后替换的最大次数，默认 0 表示替换所有的匹配。
flags : 编译时用的匹配模式，数字形式。
### 自定义排序
引入functools包

```
import functools
```
然后实现类中三个数值的排序

```
import sys
import functools

class data:
    a,b,c = 0,0,0
    def __init__(self,a,b,c):
        self.a,self.b,self.c=a,b,c
    def cmp(self,other):
        '''这种写法竟然是错误的，惊了
        if(self.a!=other.a):
            return self.a>other.a
        if(self.b!=other.b):
            return self.b>other.b
        if(self.c!=other.c):
            return self.c>other.c
        return 0
        '''
        if(self.a > other.a):
            return 1
        if(self.a < other.a):
            return -1
        if(self.b > other.b):
                return 1
        if(self.b < other.b):
            return -1
        if(self.c > other.c):
                return 1
        if(self.c < other.c):
            return -1
        return 0
    def __str__(self):
        return "a:{} b:{} c:{}\n".format(self.a,self.b,self.c)
if __name__ == "__main__":
    a= [int(i) for i in input().split()]
    b=[]
    while(a!=[0,]):
        tmp=data(a[0],a[1],a[2])
        b.append(tmp) 
        a= [int(i) for i in input().split()]
    c=sorted(b,key=functools.cmp_to_key(data.cmp))
    for temp in c:
        print(temp)

```
###  统计字符串
emmm...就是统计字符串出现的次数，忽略大小写
chr()变为char ， ord(a)字符变数字

```
input_position = input("请输入您要统计的文本文件的路径")
input_name = input("请输入您要统计的文本文件的名称")
f = open(input_position+'\\'+input_name,'r')
text=f.readlines()
dic = dict()
print(text)
for tmp in text:
    tmp = tmp.lower()
    for tmp1 in tmp:
        dic[tmp1]=dic.get(tmp1,0)+1

for i in range(0,26):
    if(chr(ord('a')+i) in dic.keys()):
        print("there has {} {} in this text".format(dic[chr(ord('a')+i)],chr(ord('a')+i)))
```


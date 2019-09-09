---
title: 7-30日报 内置函数datetime
date: 2019-07-30 10:59:46
tags: python
categories: 学习进度
---
# 7-30日报 python 内置函数datetime
## datetime模块
获取当前时间

```
from datetime import datetime
time=datetime.now()
print(time)
```
在计算机中，时间实际上是用数字表示的。我们把1970年1月1日 00:00:00 UTC+00:00时区的时刻称为epoch time，记为0（1970年以前的时间timestamp为负数），当前时间就是相对于epoch time的秒数，称为timestamp。timestamp的值与时区毫无关系，因为timestamp一旦确定，其UTC(**世界统一时间**)时间就确定了，转换到任意时区的时间也是完全确定的，比如中国北京相对于UTC时间为UTC+8，这就是为什么计算机存储的当前时间是以timestamp表示的，因为全球各地的计算机在任意时刻的timestamp都是完全相同的（假定时间已校准），但是转换到各地的时间不同。
### datetime类型和timestamp
datatime转换成timestamp

```
from datetime import datetime

time=datetime.now()
timestamp=time.timestamp()
print(timestamp)
```
timestamp转换成datetime

```
from datetime import datetime

time=datetime.now()
timestamp=time.timestamp()
time=datetime.fromtimestamp(timestamp)   #转换成本地时间
time=datetime.utcfromtimestamp(timestamp)   #转换成世界统一时间
print(time)
```
timestamp转换成datetime，这里datetime是分时区的，你可以转换为本地时间，也可以直接转换成UTC时间，即世界统一时间。
### timedelta 
使用timedelta可以对datetime对象进行任意加减时间，非常方便
```
from datetime import datetime,timedelta

time=datetime.now()
timestamp=time.timestamp()
time=datetime.fromtimestamp(timestamp)
Time1=time-timedelta(hours=8)
time=datetime.utcfromtimestamp(timestamp)
print("计算出来的utc时间和函数的时间分别为{}，{}".format(Time1,time))
```
## str与datetime互转
datetime转str使用datetime.strftime()

```
from datetime import datetime,timedelta
time=datetime.now()
str=time.strftime("%Y-%m-%d %H:%M:%S")
print(str)
#结果：2019-07-30 10:57:03
```
**常用**：str转datetime使用datetime.strptime()
```
from datetime import datetime,timedelta

time=datetime.now()
str=time.strftime("%Y-%m-%d %H:%M:%S")
time=datetime.strptime(str,"%Y-%m-%d %H:%M:%S")
print(time)
```
## 计算时间差
```
from datetime import datetime
a=input("输入小日期：")
b=input("输入大日期：")
time1=datetime.strptime(a,"%Y %m %d")
time2=datetime.strptime(b,"%Y %m %d")
time3=time2-time1
print(time3)
'''
结果:
输入小日期：2016 4 1
输入大日期：2019 7 30
1215 days, 0:00:00
'''
```
# collections模块
## Counter计数器

```
from collections import Counter

str="ghwughwkgnmsdgwiygwkfjsf"
c=Counter()
for i in str:
    c[i]=c[i]+1
print(c)
#结果：Counter({'g': 5, 'w': 4, 'h': 2, 'k': 2, 's': 2, 'f': 2, 'u': 1, 'n': 1, 'm': 1, 'd': 1, 'i': 1, 'y': 1, 'j': 1})
```


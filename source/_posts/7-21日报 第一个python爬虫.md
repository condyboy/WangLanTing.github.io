---
title: 7-21日报 第一个python爬虫
date: 2019-07-21 16:20:30
tags: python
categories: python
link: 友情链接
---
# **7-21日报**
### 1、学习了python 面向对象，有关类的基本语法
### 2、学习了python正则表达式re模块中的match、search、sub方法
### 3、学习了python简单爬虫。。。。只能爬个文本啥的，用requests和BeautifulSoup库，改天再学scrapy...

```
import requests
from bs4 import BeautifulSoup
import re

url="https://www.runoob.com/python3/python3-examples.html"
html=requests.get(url)
b=BeautifulSoup(html.text.encode('iso-8859-1').decode('utf-8'),"html.parser")
tmp=b.find_all("div",class_="article-intro")
#print(tmp)
b=BeautifulSoup(str(tmp),"html.parser")
tmp=b.find_all("a")
result_text=[]
for temp in tmp:
    result=re.search(r"Python\s(.*).*</a>",str(temp),re.M)
    print(result.group(1))
    result_text.append(result.group(1))

```
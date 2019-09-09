---
title: python爬虫入门
date: 2019-07-26 17:16:06
tags: python
categories: python
---
# python爬虫入门
## 爬虫步骤
1、请求：通过http向目标站点发送requests请求，请求可以包含headers等内容
2、响应：服务器返回response,包含html页面或json数据等
3、解析：通过正则表达式等解析数据，提取出有用数据
4、存储：以文件或者数据库等方式存储返回的信息
## get请求与post请求
get请求（请求数据）参数信息全部包含在url里面，而post（提交表单参数）的参数会以数据的方式传入，并且不会显示在url上，也不会被浏览器缓存
## 请求头
headers一般包含user-agent(浏览器信息)、cookies、host
“Cookies"是指服务器暂存放在你的电脑里的txt格式的文本文件资料，主要用于网络服务器辨别电脑使用。
爬取知乎页面，需要加请求头，否则会失败，审查元素，在network中找到请求request headers构造请求
```
import requests
from bs4 import BeautifulSoup

url="https://www.zhihu.com/explore"
headers={"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36"}
print(requests.get(url,headers=headers).text)
```

## response
包含响应状态（200成功、301跳转、404找不到资源）、响应头（设置cookies）、响应体（HTML页面）

## BeautifulSoup4
有四种解析方式
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019072616195627.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0OTQyNDI4,size_16,color_FFFFFF,t_70)
使用find_all函数来根据属性和标签来过滤，find是查找匹配到的第一个元素，而find_all返回的是全部的元素
1、如下例，找到textarea标签的内容

```
import requests
from bs4 import BeautifulSoup

url="https://www.zhihu.com/explore"
headers={"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36"}
response=requests.get(url,headers=headers).text
b=BeautifulSoup(response,"html.parser")
text=b.find_all("textarea")
print(text)

```
2、使用属性值来过滤标签，找到class为content的内容(class与python关键字冲突，所以使用class_来表示参数)

```
import requests
from bs4 import BeautifulSoup

url="https://www.zhihu.com/explore"
headers={"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36"}
response=requests.get(url,headers=headers).text
b=BeautifulSoup(response,"html.parser")
text=b.find_all(class_="content")
print(text)
```
3、使用select方法选择标签
id选择器#id   类选择器.class    标签选择器 ul

```
import requests
from bs4 import BeautifulSoup

url="https://www.zhihu.com/explore"
headers={"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36"}
response=requests.get(url,headers=headers).text
b=BeautifulSoup(response,"html.parser")
text=b.select("textarea")
print(text)
```

```
import requests
from bs4 import BeautifulSoup

url="https://www.zhihu.com/explore"
headers={"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36"}
response=requests.get(url,headers=headers).text
b=BeautifulSoup(response,"html.parser")
text=b.select("h2 .post-link")
print(text)
```
4、select获取属性中的值

```
import requests
from bs4 import BeautifulSoup

url="https://www.zhihu.com/explore"
headers={"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36"}
response=requests.get(url,headers=headers).text
b=BeautifulSoup(response,"html.parser")
text=b.select("a")
for a in text:
    print(a["href"])
```
5、select获取文本内容，使用get_text()方法

```
import requests
import re
from bs4 import BeautifulSoup

url="https://www.zhihu.com/explore"
headers={"User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36"}
response=requests.get(url,headers=headers).text
b=BeautifulSoup(response,"html.parser")
text=b.select("textarea")
for p in text:
    print(p.get_text())
```


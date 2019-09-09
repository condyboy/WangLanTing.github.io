---
title: BeautifulSoup
date: 2019-08-01 19:41:11
tags: other
categories: python爬虫
---
# BeautifulSoup
b=BeautifulSoup(html.text,"html.parser")
其中第一个参数需要传入stirng类型，如果是Tag类型，就需要用`str(tag)`强转
1、自动补全代码

```
soup.prettfiy()
```
2、标签选择器
直接使用`soup.tag`tag是我们要选择的标签

```
soup.title
```
选择html中的title标签，我们用.选择出来的标签的类型均为`<class 'bs4.element.Tag'>`
3、获取标签内容

```
soup.title.string
```
用string来获取内容，前提前面是Tag类型
4、Tag类型获取标签的名称

```
soup.title.name
```
结果会输出title
5、获取标签中的属性

```
soup.title['name']
soup.title.attrs['name']
```
6、获取标签的所有的子节点，返回值是列表形式，包括文本节点和标签节点，
```
soup.title.contents
```
7、获取标签的所有子节点，返回是迭代器的形式，需要通过遍历输出

```
soup.title.children
```
8、获取标签的父节点，返回的是Tag类型

```
soup.title.parent
```
9、获取标签的所有父节点，返回的是Tag类型

```
soup.title.parents
```
10、获取兄弟节点
返回的是列表的形式
```
soup.a.next_siblings #获取后面的兄弟节点
soup.a.previous_siblings #获取前面的兄弟节点
```
11、搜索
一、find方法
**find_all**查找所有符合条件的标签，并返回成一个列表
```
find_all( name , attrs , recursive , string , **kwargs )
```
选择id属性内容为lalala的标签，并返回
```
soup.find_all({attrs={"id":"lalala"}})
soup.find_all(id="lalala")
soup.find_all(class_="lalala") #注意class属性在这里需要用class_来表示
```
根据文本内容进行选择,选择文本内容为aaa的标签并返回其文本，感觉没啥用啊，emmmm...

```
soup.find_all(text="aaa")
```
**find**方法返回第一个被查找到的标签
二、select方法
通过CSS选择器选择，这里为标签选择器，为id选择器，为类选择器

```
soup.select("li #aa .abc")
soup.select_one("li #aa .abc") #返回第一个
```

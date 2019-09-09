---
title: 7-27日报 python爬虫实战练习
date: 2019-07-27 16:12:51
tags: python
categories: python
---

# 7-27周报   python爬虫
## 为什么需要selenium
许多网页是通过JS动态加载的，selenium是自动化测试工具，支持多种浏览器，主要用来解决爬虫渲染的问题
## 安装环境

```
pip install selenium
```
以及去官网下载chrome driver用来打开浏览器
emmmm....教程可以看[这里](https://www.cnblogs.com/zhaof/p/6953241.html)
## 爬取一个小说网站

```
from bs4 import BeautifulSoup
import requests,sys



class dowmloador:
    def __init__(self):
        self.completeurl='http://www.biqukan.com/'
        self.tmpurl="https://www.biqukan.com/1_1094/"
        self.nums=0
        self.urls=[]
        self.name=[]
    def downloadUrl(self):
        html=requests.get(url=self.tmpurl)
        html.encoding="gb2312"
        bs=BeautifulSoup(html.text,'html.parser')
        content=bs.find_all("div",class_="listmain")
        newbs=BeautifulSoup(str(content[0]),'html.parser')
        allUrl=newbs.find_all("a")
        self.nums=len(allUrl[15:])
        for tmp in allUrl[15:]:
            self.urls.append(self.completeurl+str(tmp['href']))
            self.name.append(tmp.string)
    def getContent(self,index):
        html=requests.get(self.urls[index])
        html.encoding="gb2312"
        bs=BeautifulSoup(html.text)
        content=bs.find_all("div",class_="showtxt")
        return content[0].text.replace('</br>','\n')
    def writer(self,name,path,text):
        write__flag=True
        f=open(path,mode='a',encoding='utf-8')
        f.write(name)
        f.writelines(text)
        f.write('\n\n')




dl=dowmloador()
dl.downloadUrl()

print("开始下载。。。")

for i in range(dl.nums):
    dl.writer(dl.name[i],r"D:\下载\a.txt",dl.getContent(i))
    sys.stdout.write("  已下载:%.3f%%" %  float(i/dl.nums) + '\r')
    sys.stdout.flush()
print('下载完成')
```
## 实战项目
利用正则表达式和requests库来抓取猫眼电影top100数据并保存，写入时将ensure_ascii=False，然后将open中的encoding设置为utf-8能够输出正确的中文字符

```
#!/usr/bin/python3
import json
from multiprocessing import Pool 
import requests
import re
from requests.exceptions import RequestException

def get_page(url):
    try:
        response=requests.get(url)
        if(response.status_code==200):
            return response.text
        else :
            return None
    except RequestException:
        return None
def analysis(html):
    pattern=re.compile('<dd>.*?board-index.*?>(\d)</i>.*?title="(.*?)".*?<img.*?alt=.*?src="(.*?)".*?star">(.*?)</p>.*?releasetime">(.*?)</p>'+
        '.*?integer">(.*?)</i>.*?fraction">(.*?)</i>.*?</dd>',re.S)
    
    content=re.findall(pattern,html)
    print(content)
    for temp in content:
        yield {
            "index":temp[0],
            "name":temp[1],
            "img":temp[2],
            "actors":temp[3].strip()[3:],
            "time":temp[4],
            "score":temp[5]+temp[6]
        }
   
def write_to_file(content):
    with open("result1.txt","a",encoding="utf-8") as f:
        f.write(json.dumps(content,ensure_ascii=False)+'\n')
        f.close()
def main(offest):
    url="https://maoyan.com/board/4"
    html=get_page(url+"?offset="+str(offest))
    for items in analysis(html):
            write_to_file(items)
if __name__ == "__main__":
    '''
    for temp in range(10):
        html=get_page(url+"?offset="+str(temp*10))
        for items in analysis(html):
            write_to_file(items)
            '''
    #利用线程池抓取数据，提升速度
    pool=Pool()
    pool.map(main,[i*10 for i in range(10)])

```



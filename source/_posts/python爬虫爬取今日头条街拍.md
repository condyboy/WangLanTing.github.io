---
title: python爬虫爬取今日头条街拍
date: 2019-07-29 21:41:03
tags: python
categories: 学习进度
---
# python爬虫小实战
## 自己的小练习
菜鸟教程的一些联系题
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
## 抓取今日头条街拍的图片
首先检查元素，分析请求中有没有包含你想要抓取的数据，如果没有数据，那么有可能是ajax请求
Ajax全称为Asynchronous JavaScript and XML，即异步的JavaScript和XML，如上所言，Ajax请求就是在网页加载完毕后再向服务器请求的数据，服务器会返回一个或者多个URL，里边携带这些数据，这些数据通常以JSON格式存在（后边会讲什么是JSON），通俗点讲，有时候网页会有下滑之类的选项，本来数据不会显示，鼠标下滑后就会出现新的数据而且不会刷新页面，这就是通过Ajax请求后显示的数据。
通过network中的XHR中是否有我们想要的数据，如果有，那么我们想要的数据可能是通过json来加载的，通过解析json.loads(html)来拿到json数据，并解析，ajax请求鼠标下滑之后会出现新的数据，这里offset的值以20的大小来增长。
实现参考[这里](https://blog.csdn.net/weixin_42359480/article/details/88915797)
```
import requests
from urllib.parse import urlencode
import json
from requests.exceptions import RequestException
import os
from hashlib import md5

path=os.getcwd()


def get_page(offest,keyword):

        headers={
                "user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.142 Safari/537.36",
                "cookie":"tt_webid=6718643773326738952; WEATHER_CITY=%E5%8C%97%E4%BA%AC; tt_webid=6718643773326738952; csrftoken=b595c67b0e1dcf9cae8414a80d1d6129; UM_distinctid=16c37e9ef1716c-0d23e69f99fbb8-c343162-144000-16c37e9ef183fb; __user_from=mobile_qq; __tasessionId=ars5fa5341564389009880; CNZZDATA1259612802=1553206227-1564301942-https%253A%252F%252Fwww.toutiao.com%252F%7C1564386774; s_v_web_id=28cf25b2e4e150dfffee289bdad1dec4"
        }
        data={
        "aid": 24,
        "app_name": "web_search",
        "offset": offest,
        "format": "json",
        "keyword": keyword,
        "autoload": "true",
        "count": "20",
        "en_qc": 1,
        "cur_tab": 1,
        "from": "search_tab",
        }
        url="https://www.toutiao.com/api/search/content/?"+urlencode(data)
        try:
                response=requests.get(url,headers=headers)
                if(response.status_code==200):
                        print("请求成功")
                        #使用response.encoding来改变爬虫的中文编码，通过document.charset可以查看网页编码
                        response.encoding="utf-8"
                        return response.text
        except RequestException:
                print("出现异常")
                return None


def parse_page_index(html):
        data = json.loads(html)
        if data and "data" in data:
                for i in data.get("data"):
                        if(i.get("title")==None):
                                continue
                        #获取img_list
                        img_list=i.get("image_list")
                        if not os.path.exists(path+'\picture\{}'.format(i.get("title"))):
                                os.mkdir(path+'\picture\{}'.format(i.get("title")))
                        #图片路径
                        pic_file=path+'\picture\{}'.format(i.get("title"))
                        for img in img_list:
                                littleimg=img.get("url")
                                imgcontent=requests.get(littleimg)
                                #图片名称为图片内容的md5码
                                img_path=pic_file+'\{}'.format(md5(imgcontent.content).hexdigest())+".jpg"
                                if not os.path.exists(img_path):
                                        with open(img_path,"wb") as f:
                                                f.write(imgcontent.content)
                                                print("写入照片{}成功".format(pic_file+'\{}'.format(md5(imgcontent.content).hexdigest())+".jpg"))
                                                f.close()
                        
                        

def main():
        print(path)
        for i in range(0,100,20):
                html=get_page(i,"街拍")
                parse_page_index(html)
                        
if __name__ == "__main__":
    main()

```

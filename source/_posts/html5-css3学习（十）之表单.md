---
title: html5+css3学习（十）之表单
date: 2019-08-06 21:47:28
tags: html&css
categories: html&css
---
# html5+css3学习（十）之表单
## 表单
表单的作用就是用户提交信息给服务器
使用form标签创建表单，标签中的action属性，（属性指向的是服务器的地址），提交表单时会提交到action属性对应的地址。
使用input创建提交表项，并且必须指定name属性以便获取提交的表项
```
<form>
    <input name="" type="text" value="这里是默认值"/>
    <input type="submit" value="显示的值"/>
</form>
```
## CSS Hack
一些特殊的情况，我们的代码只在某些特殊浏览器中执行，而不需要在其他浏览器中执行，这时可以使用Hack
条件Hack只对IE浏览器有效，可以使用条件Hack为IE6等特殊浏览器**设置单独的css样式**，其他的浏览器都会将它识别为注释，IE10及以上的浏览器已经不支持这种方式
```
<!--[if IE 6]>
       <p>只在IE6中显示</p>
<![endif]-->

<!--[if lt IE 9]>
       <p>在IE9及以下浏览器显示</p>
<![endif]-->
```
## 框架集frameset
框架集和内联框架类似，都是用于在一个页面引入其他的外部页面，框架集可以引入多个页面，而内联框架只能引入一个，**在h5中推荐使用框架集，而不适用内联框架**
使用frameset来创建一个框架集，但是frameset和body不能同时出现在一个页面中，使用框架集就不能使用body
frameset属性：排列方式必须选择一个
rows:指定框架集中的所有框架，按行排列
cols:指定框架集中的所有框架，按列排列
frameset中可以嵌套frameset
frameset和iframe一样，**里面的内容不能够被搜索引擎检索**，并且只能引入页面，不能有自己的页面，每个引入的页面还要单独发起请求。
```
//其中*就是40%
<frameset rows="30% * 30%">
     <frame src="引入的页面地址"/>
     <frame src="引入的页面地址"/>
     <frame src="引入的页面地址"/>
</frameset>
```

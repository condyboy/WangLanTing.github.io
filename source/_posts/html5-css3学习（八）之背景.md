---
title: html5+css3学习（八）之背景
date: 2019-08-06 17:59:18
tags: html&css
categories: html&css
---
# html5+css3学习（八）之背景
## 设置背景透明
如果要同时兼容浏览器，需要将以下两个属性都写上
1、opacity：不支持IE8以下的浏览器
```
opacity：用来设置元素背景的透明
它是0~1之间的值，0是完全透明，1是完全不透明
```
2、IE8以下使用filter属性
此时opacity的值在0~100之间
```
filter:alpha(opacity=50);
```
## 背景
可以同时为背景设置背景颜色和背景图片，背景颜色在背景图片后。
**如果使用简写属性，那么就只整合到一个简写属性中，以免其他被覆盖**
```
//简写属性，可以设置任何与background相关的样式，没有顺序和数量要求，不设置的会采用默认值
background:
//设置背景图片，如果背景图片大于元素，就会显示图片左上角，如果小于就会重复平铺。
background:url(position);
//可选值 repeat no-repeat(不重复) repeat-x(横向重复) repeat-y(纵向重复)
background-repeat:
//设置背景图片位置
background-positsion:
//设置背景图片是否随页面一起滚动,scroll(背景图片随着滚动)、fixed(背景图片固定，一直相对于浏览器窗口，一般设置给body)
background-attachment:
```
`background-positsion`
1、该属性使用top right left bottom center中的两个值来指定一个背景图片的位置
top left 左上
bottom right 右下
如果只给出一个值，则第二个值默认是center
2、`background-positsion:apx bpx`直接指定向右向下的偏移量
## 按钮样式问题
当按钮选择图片作为背景时，不同状态的按钮为了防止异步加载时的闪烁，需要使用一张背景图片（图片整合技术），需要通过`background-positsion`来根据需要移动按钮背景的方向


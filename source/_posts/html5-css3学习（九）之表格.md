---
title: html5+css3学习（九）之表格
date: 2019-08-06 20:44:38
tags: html&css
categories: html&css
---
# html5+css3学习（九）之表格
## 表格
tr表示行，td表示列，th表示表头内容，table是块元素
```
合并单元格：纵向：rowspan  横向:colspan
<table>
    <tr>
        <th colspan="2"></th>
    </tr>
    <tr>
        <td colspan="2"></td>
    </tr>
</table>
```
## 表格边框

```
//table和td之间默认有一个距离，通过在table中的border-spacing属性来设置距离
border-spacing:0px;
//设置合并td和table边框，collapse是合并边框，设置collapse，边框距离属性失效
border-collapse:collapse;
```
隔行变色，IE8以上支持

```
tr:nth_child(odd){
        backgrond-color:#000
}
```
## 长表格
如果表格中没有写tbody，浏览器会自动添加，tfoot的内容永远显示在表格的底部，thead的内容永远显示在表格顶部，tbody的内容永远显示在表格中间。

```
<table>
      <thead></thead>
      <tbody></tbody>
      <tfoot></tfoot>
</table>
```
## 用table解决父子元素外边距重叠
用table解决父子元素外边距重叠
```
.div1:before{
    content="";
    display=table;
}
```
## 解决高度塌陷和父子元素垂直外边距重叠问题
完美写法

```
.div1:before,.div1:after{
      content="";
      display=table;
      clear=both;
}
```

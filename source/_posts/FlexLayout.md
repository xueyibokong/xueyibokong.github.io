---
title: FLEXBOX(弹性布局)
date: 2018-05-26 21:58:22
tags: 
    - 前端
    - 布局
categories: 
    - 技术
---
# flex的概念
flex是一种可伸缩的布局方式，由css3提供，主要分为两层结构（可重复嵌套），伸缩容器（flex container）和伸缩项目（felx item），为包含关系。可以轻松解决一些复杂的css布局，如自动分割，垂直居中等。在考虑大规模应用此项技术之前可以进行[兼容性查阅](https://caniuse.com/)，视情况而定。
# flex特性
通过display:flex;便可以定义一组flex模型了；
```
display: -moz-box; /* Firefox */
display: -ms-flexbox; /* IE10 */
display: -webkit-box; /* Safari */
display: -webkit-flex; /* Chrome, WebKit */
display: box;
display: flexbox;
display: flex;
```
设为Flex布局以后，子元素的`float`、`clear`和`vertical-align`属性将失效。

#### 相比较传统web布局而言，flex布局有很多优点：
传统布局是自上而下的线性布局，对于内联元素是从左到右排版，那么flex则是根据flex-flow流布局。
# flex图解
![flex盒子模型图](https://raw.githubusercontent.com/xueyibokong/BlogImages/master/res/flexModel.jpg)

<center>flex盒子模型图</center>

![flex属性图](https://raw.githubusercontent.com/xueyibokong/BlogImages/master/res/flexbox.jpg)

<center>flex属性图</center>

# flex属性工具
你可以通过这个[demo](../../../../html/Flexbox.html)来测试flexbox的每一个属性从而更加方便的记忆理解。
另外，可以试试这个[FlexboxFroggy](http://flexboxfroggy.com/#zh-cn)。

请用支持vh,vw的浏览器调试，在测试属性时候请结合着调节浏览器窗口size。
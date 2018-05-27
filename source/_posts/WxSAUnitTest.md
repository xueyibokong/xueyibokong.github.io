---
title: 微信小程序socket、重力感应、上拉加载下拉刷新、折线表、拖拽
date: 2018-05-27 00:03:58
tags: 
    - 前端
    - 微信小程序
categories: 
    - 技术
---
[项目github地址](https://github.com/xueyibokong/wxSAUnitTest)
# socket 【多人群聊，用户头像是微信用户信息】
利用微信的socket与后端完成通道消息传输。
<center>![](https://raw.githubusercontent.com/xueyibokong/BlogImages/master/res/sayhello.gif)</center>

# 重力感应 【云雾动态壁纸】
利用重力感应将x轴y轴的值动态追加到背景图云雾（3层云雾图）的left和top上，达到摇动手机背景透视的效果。

#上拉加载下拉刷新
此功能为列表常用功能，微信小程序在实现此功能上目前又两种体现在代码中"pages/scrolltest/scrolltest",
"pages/scroll/scroll"两个page中，具体效果亲测可知。需要注意的是：

## 1、pages/scrolltest/scrolltest
scroll-view组件提供的边界判断是无回弹效果的，并且当滚动条已达边界时再向零界方向滑动是不会再次触发边界事件的（不适合做下拉刷新可以做瀑布流）。
## 2、pages/scroll/scroll
而通过onPullDownRefresh事件监听page下拉的动作则又回弹并会重复触发（适合做下拉刷新），onReachBottom事件可以多次触发但同样无回弹（不过可以通过监听到的事件自己做回弹效果，适合做上拉刷新）。

# 折线表 【chart】
通过canvas绘制折线图根据y轴数据最大值计算y轴范围。
`注意：` 小程序的canvas重绘制无需清除画布，h5则需要。
</center>![](https://raw.githubusercontent.com/xueyibokong/BlogImages/master/res/linechange.gif)</center>

# 拖拽 【列表左滑动】
<center>![](https://raw.githubusercontent.com/xueyibokong/BlogImages/master/res/leftDrag.gif)</center>

# 二维数组实现单选多选模块
<center>![](https://raw.githubusercontent.com/xueyibokong/BlogImages/master/res/multArray.gif)</center>

# 地图
<center>![](https://raw.githubusercontent.com/xueyibokong/BlogImages/master/res/map.gif)</center>

---
title: 布局中长度单位的灵活应用
date: 2018-05-26 22:56:20
tags: 
    - 前端
    - 布局
categories: 
    - 技术
---
如果说把RWD和AWD比作网页布局树干的话，那这些单位便是枝叶了，只有很好的结合应用，才能将页面灵活起来。
# 先列举一下css中的长度单位:

长度单位	| 版本 |	描述 
------- | ------ | -------- 
文本相对长度单位 Font-relative Length Units | |
em	| CSS1 | 相对于当前对象内文本的字体尺寸
ex	| CSS1 |	相对于字符“x”的高度。通常为字体高度的一半
ch	| CSS3 |	数字“0”的宽度
rem	| CSS3 |	相对于根元素(l元素)font-size计算值的倍数
视口相对长度单位 Viewport-relative Length Units | |
vw	| CSS3 |	相对于视口的宽度。视口被均分为100单位的vw
vh	| CSS3 |	相对于视口的高度。视口被均分为100单位的vh
vmax	| CSS3 |	相对于视口的宽度或高度，总是相对于大的那个。视口的宽度或高度被均分为100单位的vmax
vmin	| CSS3 |	相对于视口的宽度或高度，总是相对于小的那个。视口的宽度或高度被均分为100单位的vmin
绝对长度单位 Absolute Length Units | |
cm	| CSS1 |	厘米
mm	| CSS1 |	毫米
q	| CSS3 |	1/4毫米（quarter-millimeters）; 1q = 0.25mm
in	| CSS1 |	英寸（inches）; 1in = 2.54cm
pt	| CSS1 |	点（points）; 1pt = 1/72in
pc	| CSS1 |	派卡（picas）; 1pc = 12pt
px	| CSS1 |	像素（pixels）; 1px = 1/96in

# 这里重点会说到px，%，em，rem，vh，vw等在布局中的应用场景

# PX：
在web开发中对绝对长度单位的应用一般都是PX，px其实为css的逻辑像素，针对移动端开发的同学还需要注意逻辑像素和物理像素在开发中的区别和开发受到的影响。
# %：
严格意义上讲%也能算作相对长度单位，相信大部分前端同学都会喜欢%来控制页面元素占位，%继承于父辈元素，可用于很多css属性，如：字体（font），尺寸（dimension），外补白（margin），内补白（padding），定位（position），变换tronsform等。前文响应式分类中的DEMO就用到了%，可回顾。
# EM REM:
em，rem这两个单位需要放在一起比较，em是当前dom对象的字体尺寸，rem是根元素字体尺寸即root em；

往往响应式设计都会配合百分比，em和rem来用的实现页面跨端适应。

这里写个demo，用来描述em和rem的应用场景；
```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style type="text/css">
			@media only screen and (max-width: 320px) {
				html{
					font-size: 64%;
				}
			}
			@media only screen and (min-width: 320px) and (max-width: 375px){
				html{
					font-size: 76%;
				}
			}
			@media only screen and (min-width: 375px) and (max-width: 414px){
				html{
					font-size: 84%;
				}
			}
			@media only screen and (min-width: 414px) and (max-width: 768px){
				html{
					font-size: 92%;
				}
			}
			@media only screen and (min-width: 768px) and (max-width: 1024px){
				html{
					font-size: 100%;
				}
			}
			@media only screen and (min-width: 1024px){
				html{
					font-size: 108%;
				}
			}
			.bigBox{
				font-size: 3rem;
				border: 1px solid #ccc;
			}
			.bigFont{
				font-size: 1em;
			}
			.middleFont{
				font-size: 0.75em;
			}
			.smallFont{
				font-size: 0.5em;
			}
		</style>
	</head>
	<body>
		<p>
			下面框内元素用到的是em，三行的字体比例是固定的。与root的font-size是rem单位
		</p>
		<div class="bigBox">
			<p class="bigFont">
				bigFont
			</p>
			<p class="middleFont">
				middleFont
			</p>
			<p class="smallFont">
				smallFont
			</p>
		</div>
	</body>
</html>
```
# VW VH:
这两个单位是针对视窗而定的相对单位，相比较em，rem，%来说更加的直观粗暴，而且我们知道在线性布局中，我们一般不会给html和body固定的高度这样的话想要让元素具有height上的百分比就实现不了了（当然js参与除外），这时vh就能完美解决。
复制如下code，放在新建的html文件中测试：
```html
<div style="height: 80vh; border: 1px solid #333333;margin-top: 10vh;"></div>
```
就可以实现垂直居中。
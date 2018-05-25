---
title: 前端基础知识点整理
date: 2018-05-25 10:30:12
tags: 
	- 前端知识点
	- 整理
categories: 技术
---
# 前端面试题

### 一、布局题
`模态窗布局`要求：遮罩满屏、内容部分垂直水平居中且不定宽高，请写出标签及样式。进阶要求：内容容器内文案较少时（一行）居中对齐，较多时（多行换行）居左对齐。以上要求不得有js参与。
#### dom
```
<div class="mask">
	<div class="modal-box">
		...
		<div class="modal-box-body">
			<div class="center-body">
				测试文案...
			</div>
		</div>
		...
	<div>
</div>
```
#### 样式一：
```
	//绝对定位 + transform
	.mask{
		position:fixed;
		top:0;bottom:0;left:0;right:0;
	}
	.modal-box{
		position:absolute;
		top:50%;left:50%;
		transform:translate(-50%,-50%);
	}
	解释：
```
#### 样式二：
```
	//操作行内块级元素
	html,body{
        width:100%;
        height:100%;
    }
    .mask{
        width:100%;
        height:100%;
        text-align: center        
    }
    .mask:after{
        content:"";
        display:inline-block;
        height:100%;
        width:0;
        vertical-align:middle;
    }
    .modal-box{
        display:inline-block;
        margin:0 auto;
    }
```
#### 样式三：
```
	//flex布局（主要是实现垂直居中）
	.mask{
        position:fixed;
        top:0;bottom:0;left:0;right:0;
        display: flex;
        align-items:center;
        justify-content: center;
    }
    .modal-box{
        flex: 1;
        max-width: 80%;

    }	
```
#### 进阶要求答案
```
	.modal-box-body{
		text-align:center;
	}
	.center-body{
		display:inline-block;
		text-align:left;
	}
```

`自适应布局` 要求：左边固定宽，右边自适应。以上要求有js参与。
#### dom
```
	<div class="box">
        <div class="left"></div>
        <div class="right">
            ...
        </div>
    </div>
```
#### 样式一
```
	//定位 + padding-left
	.box{
        position: relative;
        height: 35px;
    }
    .box>div{
        height: 100%;
    }
    .left{
        position: absolute;
        width: 120px;
        background: red;
    }
    .right{
        width: 100%;
        padding-left: 120px;
        background: greenyellow;
    }
```
#### 样式二
```
	//flex 布局，左边定宽，右边弹性填充
	.box{
        display: flex;
        height: 35px;
    }
    .box>div{
        height: 100%;
    }
    .left{
        width: 120px;
        background: red;
    }
    .right{
        flex: 1;
        background: greenyellow;
    }
```
#### 样式三
```
	//table 布局
	.box{
        display: table;
        width: 100%;
        height: 35px;
    }
    .box>div{
        display: table-cell;
        height: 100%;
    }
    .left{
        width: 120px;
        background: red;
    }
    .right{
        background: greenyellow;
    }
```
#### float方案
```
	//布局
	<div class="box">
  		<div class="left"></div>
   	<div class="right">
    		...
   	</div>
	</div>
	//样式
	.box{
	   height: 35px;
	}
	.box>div{
	   height: 100%;
	}
	.left{
	   float:left;
	   width: 120px;
	   background: red;
	}
	.right{
		//不能给100%宽度，会超出父元素出现滚动条
		padding-left:120px;//必须给padding-left值，如果没有则只有子行内元素会被前兄弟节点挤到右面
	   background: greenyellow;
	}
```
### css BFC、IFC、GFC、FFC
```
	BFC（block fomatting context）是web布局的块级元素格式化上下文。
	BFC布局规则：
	.	内部的Box会在垂直方向，一个接一个地放置。
	.	Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
	.	每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
	.	BFC的区域不会与float box重叠。
	.	BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
	.	计算BFC的高度时，浮动元素也参与计算
	以下方式可以创BFC
	.	根元素
	.	float属性不为none；float本身会对上下文影响，但是其子节点对其外部不会有影响
	.	position为absolute或fixed
	.	display为inline-block, table-cell, table-caption, flex, inline-flex
	.	overflow不为visible
	举例：
		`自适应两栏布局`
	    .aside {
	        float: left;//生成NFC
	        background: #f66;
	    }
	    .main {
	        overflow:hiddle;//生成NFC
	        background: #fcc;
	    }
	    <div class="aside">asfsdf</div>
	    <div class="main">1234242</div>
	    
	`overflow:hiddle清除浮动`
		利用了BFC包裹的特性，计算内部高度并且不影响包裹快外部元素，所以可以解决浮动导致包裹节点的塌陷问题。
	`利用NFC的margin边界接触规则解决内部元素margin包裹元素的位置影响`
		<div class="body">
		  <div>
		    asdf
		  </div>
		</div>
		.body{
		  background:blue;
		  overflow:hidden; //生成nfc，效果 === float:left;
		}
		.body>div{
		  border:1px solid red;
		  margin-top:30px;
		}
		
	IFC (inline formatting context)是web布局的内联元素格式化上下文。
		IFC用来包裹内联元素，其高度为内联元素最高所决定，给IFC设置text-align:center;
	会使其内部元素水平居中。
		把IFC中最高元素基线设置为vertical-align:middle能使IFC内部元素垂直居中。
	
	GFC (GridLayout Formatting Contexts) 网格布局格式化上下文 display:grid
	
	FFC (Flex Formatting Contexts)自适应格式化上下文 display:flex
		
```
### 用dom节点绘制一个微信logo
`css 绘制icon`
```
	<div class="wx-logo">
	  <div>
	    <span></span>
	  </div>
	  <div>
	    <span></span>
	  </div>
	</div>
	
	.wx-logo{
	  font-size:20px;
	  width:10em;
	  height:10em;
	  border:1px solid red;
	  margin:30px;
	  padding:1em .7em;
	  box-sizing:border-box;
	}
	.wx-logo>div:nth-of-type(1){
	  background:linear-gradient(#98ea61,#78b94c);
	  width:7.5em;
	  height: 6em;
	  border-radius:50%;
	}
	.wx-logo>div:nth-of-type(2){
	  background:linear-gradient(#f2f2fa,#c3c3c3);
	  width:5.8em;
	  height: 4.4em;
	  border-radius:50%;
	  transform:translate(50%,-77%);
	}
	.wx-logo>div{
	  position:relative;
	}
	.wx-logo>div>span{
	  position:absolute;
	}
	.wx-logo>div:nth-of-type(1)>span{
	  left:15%;
	  top:calc(100% - 0.67em);
	  transform:rotate(40deg);
	  border-top:1em solid #78b94c;
	  border-left:0.3em solid transparent;
	  border-right:0.3em solid  transparent;
	  border-bottom:0 solid transparent;
	}
	.wx-logo>div:nth-of-type(2)>span{
	  right:15%;
	  top:calc(100% - 0.6em);
	  transform:rotate(-40deg);
	  border-top:1em solid #c3c3c3;
	  border-left:0.3em solid transparent;
	  border-right:0.3em solid  transparent;
	  border-bottom:0 solid transparent;
	}
	.wx-logo>div:nth-of-type(1):before{
	  content:'';
	  display:block;
	  width:0.7em;
	  height:0.7em;
	  border-radius:50%;
	  background:#447921;
	  transform:translate(2.1em,1.5em);
	}
	.wx-logo>div:nth-of-type(1):after{
	  content:'';
	  display:block;
	  width:0.7em;
	  height:0.7em;
	  border-radius:50%;
	  background:#447921;
	  transform:translate(4.7em,0.73em);
	}
	.wx-logo>div:nth-of-type(2):before{
	  content:'';
	  display:block;
	  width:0.56em;
	  height:0.56em;
	  border-radius:50%;
	  background:#888888;
	  transform:translate(1.6em,1.3em);
	}
	.wx-logo>div:nth-of-type(2):after{
	  content:'';
	  display:block;
	  width:0.56em;
	  height:0.56em;
	  border-radius:50%;
	  background:#888888;
	  transform:translate(3.5em,0.7em);
	}
```
### 类型转换
```
	强制转换：
	转换为数值类型：Number(mix)、parseInt(string,radix)、parseFloat(string)
	转换为字符串类型：toString(radix)、String(mix)
	转换为布尔类型：Boolean(mix)
	
	Boolean('') //false
	Boolean(0);	//false
	Boolean(NaN); //false
	Boolean(undefined) //false
	Boolean(null) //false
	Boolean([])	//true
	Boolean([].valueOf())//true
	Boolean([].toString()) //false
	
	隐式转换：
	++ -- - / * - 逻辑运算符 关系操作符（==）
	
	[] == []
	//false
	false == []
	//true
	[] == false
	//true
	{} == {}
	//false
```
### 数据的浅拷贝和深拷贝，数据深拷贝的由来以及实现方式
```
	深拷贝是针对于引用数据类型的，由于引用数据类型会将数据本身存储到堆中，将指针（指向堆里数据的地址）存在栈内。浅拷贝可实现不同指针指向同一数据，并不能将数据本身复制，而深拷贝会在内存中开辟新空间来实现数据和指针的全部复制。
	浅拷贝的方式有赋值，传参等。
	深拷贝的方法有：
	1、JSON.stringify()/JSON.parse();
	2、对于一位json可以有现成的api比如array的slice、concat等，凡是可以生成新json的api都可以。
	3、对多维json进行遍历加递归，判断条件为基本数据类型后结束递归并赋值。
	
	`应用`
	深拷贝和浅拷贝在开发中需应用得当。
	浅拷贝可以节省内存开销，并使一组数据在多端程序操作后保持同步，函数传参常用。
	深拷贝会耗费一定内存，但是在特定的情景下是必要的，比如封装开发，对初始化数据进行【备份】等。
```

### this
```
	`browser`
	console.log(this === window)//true
	`node`
	console.log(this === global)//true
	
	this指的是，调用函数的那个对象。
	call、apply会改变this的指向
```
### 继承
`构造函数`
```
	function _A (){
		this.name = 'A';
		this.arr = [1,2,3]
	}
	_A.prototype.o = {'o':'oo'}
	var A = new _A();
	A.name;//A
	var B = new _A()
	B.name;//A
	A.name === B.name;//true
	A.arr === B.Arr;//false
	A.o === B.o; //true 注意：通过构造函数new出来的对象，继承其构造函数的原型
```
`call、apply`
```
	function _people(){
		this.body = {
			head : 'one',
			hands: 'tow',
			foots : 'tow'
		}
		this.ability = {
			language : true,
			fly : false
		}
	}
	_people.prototype.getTheNmae = function(){
		return this.theName
	}
	function _man (){
		_people.call(this)
	}
	var people = new _people();
	var zhangsan = new _man();
	zhangsan.body;//{head: "one", hands: "tow", foots: "tow"}
	zhangsan.body === people1.body//false
	zhangsan.theNmae = '张三'
	zhangsan.getTheName; //undefined 注意：call、apply造成的继承是不会继承父类的原型
```
`原型继承`
```
	function _A(){
		this.obj = {"a":"aa"}
	}
	_A.prototype.arr = [1,2,3];
	
	var A = new _A();
	var B = new _A();
	A.arr;//[1,2,3]
	A.arr === B.arr;//true
	A.obj === B.obj//false
```
`原型链继承`
![](https://pic1.zhimg.com/e83bca5f1d1e6bf359d1f75727968c11_r.jpg)
```
	`关系如上图：`
	
	`范式如下：`
	function _a(){
		this.name = 'a'
		this.num = [1,2,3]
	}
	_a.prototype.getNum = function(){
		return this.num
	}
	_a.prototype.getName = function(){
		return this.name
	}
	function _b(){
		this.name = 'b'
	}
	_b.prototype = new _a()
	_b.prototype.constructor = _b;
	var b = new _b();
	b.getNum();//[1,2,3]	 
	b.getName();//'b' 注意：对象读取属性依据继承链向上读取
	
	`__ptoto__`
	1.对象有属性__proto__,指向该对象的构造函数的原型对象。
	2.方法除了有属性__proto__,还有属性prototype，prototype指向该方法的原型对象。
	
	`用途`
	实现继承的主要方法
	常用于以基类、二级类、...方式开发js工具库、游戏。
```
### 闭包
```
	function addFn(){
		var z = 0
		return function _(){
			console.log(z++)
		}
	}
	var add = addFn();
	add()//0
	add()//1
	add()//2
	add()//3
	
	注解：将‘addFn’执行后赋值给’add‘字面量，‘add’接收到返回的‘_’,'_'内部引用‘addFn’作用域中的变量，由于’add‘未销毁，所以'_'与‘addFn’的上下文关系就未被销毁。add()便是‘addFn’作用域里的‘z’，实现了内部变量外部访问
```
### 动画
```
	transition（过度）（css实现）
	animation （动画）（css实现）
	setInterval（定时器）（js 定时器）
	requestAmationFrame（js帧绘画）实现方式是递归执行
```
### 什么是动画队列？动画队列有哪些方式？
`动画题` 
```
	 `jquery`
	 $('#box').animate({'left':'100px'},1000).animate({'width':'200px'},1000).animate({'left':'0'},1000).animate({'width':'100px'},1000);
	 `原生js配合css`
	 监听transitionEnd、animationEnd事件，在回调中做动画
	 利用transition的等待时间，或者用animation分步动画
```
### 前端存储 数据持久化
`Session Storage 会话存储`
```
	localStorage生命周期是永久，这意味着除非用户在浏览器提供的UI上清除或者localStorage.clear()清除，否则这些信息将永远存在。存放数据大小为一般为5MB,而且它仅在客户端（即浏览器）中保存，不参与和服务器的通信。
```
`Local Storage 本地存储`
```
	sessionStorage仅在当前会话下有效，关闭页面或浏览器后被清除。存放数据大小为一般为5MB,而且它仅在客户端（即浏览器）中保存，不参与和服务器的通信。源生接口可以接受，亦可再次封装来对Object和Array有更好的支持。
```
`作用域不同`
```
	不同浏览器无法共享localStorage或sessionStorage中的信息。相同浏览器的不同页面间可以共享相同的localStorage（页面属于相同域名和端口），但是不同页面或标签页间无法共享sessionStorage的信息。这里需要注意的是，页面及标签页仅指顶级窗口，如果一个标签页包含多个iframe标签且他们属于同源页面，那么他们之间是可以共享sessionStorage的。
```
`Cookie`
```
	生命期为只在设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭。 存放数据大小为4K左右 。有个数限制（各浏览器不同），一般不能超过20个。与服务器端通信：每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题。但Cookie需要程序员自己封装，源生的Cookie接口不友好(http://www.jb51.net/article/6... 
)。
```
`DB`
```
	web sql、indexDB
```
### 和服务端的数据交互
`http`
```
	methods : get、post、put、option、delete...
	状态码 : 200（正常）、300（重定向）、400（客户端错误）、500（服务端错误）
	http是无状态、结合cookie和session描述http会话过程： Session是怎样做到会话身份识别的呢？首先，用户端向服务端发送一个请求，服务端接收到请求后，初始化会话，生成相应的会话信息，核心是会话ID，把会话ID发送给客户端，客户端接收到这个会话ID，把它存储起来，下一次发送请求的时候，附带着这个会话ID一起发送给服务端，服务端只要根据这个会话ID，就知道是谁了。这个会话ID，就像我们的身份证号码，一直伴随终生。核心：服务端如何生成这个会话ID，客户端怎样存储这个会话ID。 
```
`ajax 上送数据格式`
```
	Query String（get请求）；
	Request Payload（post请，Content-Type:text/plain;charset=UTF-8 || application/json; charset=utf-8）；
	Form Data（Content-Type:application/x-www-form-urlencoded ）
```
`跨域`
```
	`跨域的由来`
	跨域是浏览器对不同域名下的站点间脚本数据交互的保护措施，具体表现在ajax脚本对不同域名下的服务发出的数据请求和不同站点间的通信会被跨域拦截。
	
	`jsonp 解决一级域跨域`：原理，简单实现
	原理是客户端创建script标签src请求服务端返回的一个js文件，在这个js文件中上面是要返回的json数据，下面是一个callback函数的执行并将上面的数据作为实参回传，callback函数名在客户端请求时一query string方式上传。客户端在这个js文件加载成功后编写回调具名函数，函数名为上面提到的callback。
	`cors 解决一级域跨域`: 原理，实现。
	ajax发起后requestHeader设置withCredentials和crossDomain为true，表明ajax要发起跨域请求了，在服务端返回的responseHeader中需要设置ajax脚本执行所在域的域名为信任站点，即设置res.header("Access-Control-Allow-Origin",req.headers.origin);
	`服务器代理 解决一级域跨域`
	由于跨域是浏览器对ajax做的限制，因此服务端请求服务端的数据是不存在跨域的，所以用同域下的服务求代理客户端去请求跨域的服务数据是可以的。
	
	`二级域名跨域`
	开发中时长遇到二级域名跨域的场景，比如：iframe间通信，cookie跨域共享等，这些场景多数可以通过document.domain和服务端设置cookie的cross来解决。
```
### 事件
`dom 0级时间`
```
	<div onclick="alert(1)">点击</div>
```

`dom 2级事件`
```
	addEventListener
```
`事件流`
```
	事件捕获、事件元、事件冒泡
```
`事件对象的应用`
```
	e.target 实现事件委派
	e.clientX,e.clientY...
```
`移动端事件点透原因及处理`
```
	原因：移动端触屏会发生两类事件，touch和click。当情景为两个非后代继承关系的dom节点重叠，z-index高的节点挂touch事件并当事件执行后将此dom从dom树种擦除（实际便是引起dom树重绘并且此渲染位置被后者占用），另一个挂click事件时会出现点透现象。
	原理：移动端click触发会有300 ms的延迟，当上层touch执行完毕后会相继执行上层的click，发现点击的位置上是后者，便把click作用于后者上了。
	处理方案：禁止浏览器默认事件，e.preventDefault();
```
### 同步异步
```
	js是同步执行，但是有异步机制，定时器、计时器、ajax、promise、async函数、同异步加载等。
	setTimeout(function(){
		console.log(0)
	},100)
	console.log(1)
	setTimeout(function(){
		console.log(2)
	},0)
	for(var i = 0; i < 10000; i++){
		if(i === 9999){
			console.log(3)
		}
	}
	
	执行结果 ：
	1
	3
	2
	0
```
### 前端优化
```
	`从用户角度`
	FEcoder在设计前端优化时首先应从用户角度看，除去UE要考虑的人机交互优化外，coder更应注重软件的稳定性，易用性、好用性、安全性等。
	稳定性：不出现操作卡顿等应用不稳定现象；
	好用性：节省流量、快速调起、快速响应等直接或间接的表现；
	美观性：不出现失真、完美还原设计图、舒适的过度动画、减少页面呈现断层感；
	安全性：软件对用户信息的保密、前后端数据交互的加密、反显脱敏、高危操作保护；
	`开发者要考虑的`
	稳定性的要求决定了开发者在软件开发过程中代码的健壮和精简，软件维护措施的时效。需要coder编写规范，控制页面数据量、减少频繁的dom操作，遵循语言机制（尽量使用最切近目标的实现方式，比如能用css的就不用js），代码要易维护。
	减少http请求、静态文件客户端缓存可以节省用户流量。
	cdn和缓存服务器可以降低后台服务器压力。
	降低首屏加载量、缓存服务支持、图片懒加载、过度的页面交互等能够让应用实现快速调起并减少断层感。
	icon矢量化，扁平化的应用可以大幅度避免图片的应用。
	事件的处理和loadingUI能提高页面快速响应，比如处理点透、事件委派。
	pc端响应式布局能提高应用在多类屏的较完美呈现，移动端dpr，postcss等的结合应用可以实现多终端较完美呈现
	前端安全性在金融类应用中较为重要，https可以实现数据安全传输，用户名密码等敏感信息可以散列（md5、sha1）后存储并对比。对前端数据采集（如：表单录入）应当提高安全防护，比如：密码键盘、脱敏录入
```
### 前端可视化
```
前端可视化包括：计算机图形学（图形交互技术、真实感图形计算与显示算法、科学计算可视化、计算机动画、自然景物仿真、虚拟现实、物理现象计算等）；UE用户体验（易用、友好、美观、品牌）
div+css、svg、webgl(canvas 2D、3D)
```
`试用一种方案描述实现弹性小球落地效果`
```
	`div+css` or `svg`
	描述：利用border-radius + box-shadow等属性绘制小球，animation设置小球多次回弹动画，注意需要适当的贝瑟尔值。
	`canvas`
	描述：canvas.getContext('2d')绘制小球，利用setInterval或者requestAmationFrame函数重绘canvas实现动画，重绘算法如下：设小球质量、重力加速度、小球碰撞耗能，利用动能守恒定律计算小球在每次落地耗能后回弹最高点，再根据加速度和初速度计算出每一秒的位移。
```
### 前端开发方案
`布局自适应方案`
```
	响应式布局（栅格化布局）、flex布局、百分比线性布局、移动端rem和组件em + flexable(原理：重设dpr，重设html的font-size)
```
`工程管理`
```
	`脚手架`
	开发支持：实现热刷新、数据mock、语言解析、开发目录规范
	发布发布：编译、压缩、抽离、整理生成目录、替换cdn
```
`本地联调`
```
	本地联调解决的是跨域请求问题，如果已经cors的服务端返回则可以直接联调，否则需要架设代理服务进行代理请求。
```
>另外多人协作开发、测试支持、上线支持等

### 封装
```
	在日常开发中，时长会遇到相同或相似的场景，然而将相同或相似的代码到处搬运无疑不是一个明智的做法，这时就需要将这些相同或者相似的代码进行抽离封装。
	封装的原则是什么？
	1、低耦合高内聚，遵循此原则可以让封装的代码降低与上下游环境的耦合，解耦能增加代码的应用场景从而提高复用性。
	2、易引用(AMD、CMD、UMD)、易调用（调用方式简单）、易配置（针对封装暴露出来的能力）
	3、易维护、易拓展。
```
### 组件动手题
```
	(function(){
		
	})()
```






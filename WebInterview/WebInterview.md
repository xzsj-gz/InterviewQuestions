## 一、举例说明局部变量和隐式全局变量
```js
f1(); //调用方法 f1,方法被执行，此时 a 是局部变量，外部不能访问，b 和 c 是隐式全局变量，外部可以访问
console.log(c); // 隐式全局变量，外部可以访问，输出 2
console.log(b); // 隐式全局变量，外部可以访问，输出 2
console.log(a); // 局部变量，方法外部不能访问，报错

function f1(){ // f1 方法
	// 方法中定义的变量为局部变量，var a=b=c=2,相当于 var a=2, b=2, c=2
	// 声明变量没有 var，b=2 这样赋值的变量为隐式全局变量，在方法外部也能访问
	// 所以 a 有且仅能在方法内部访问，b 和 c 可以在方法外面访问
	var a=b=c=2 
	console.log(a); // 输出 2
	console.log(b); // 输出 2
	console.log(c); // 输出 2
}
```

******

## 二、如下分别输出什么？
```js
var name = "The Window";
var object = {
	name : "My Object",
    getNameFunc : function(){
      return function(){
        return this.name;
      };
    }
};


alert(object.getNameFunc()()); // 输出 The Window


var name = "The Window";
var object = {
	name : "My Object",
	getNameFunc : function(){
		var that = this;
		return function(){
			return that.name;
		};
	}
};
alert(object.getNameFunc()()); // 输出 My Obje
```

******

## 三、选中输入框显示对应提示的几种解决方式
```html
<html>
	<body>
		<div>
			<div style="text-align:center;">
				<p id="help">Helpful notes will appear here</p>
				<p>E-mail: <input type="text" id="email" name="email"></p>
				<p>Name: <input type="text" id="name" name="name"></p>
				<p>Age: <input type="text" id="age" name="age"></p>
			</div>
			<div>
			</div>
		</div>
	</body>
	<style>
	</style>
	<script>
		function showHelp(help) {
		  document.getElementById('help').innerHTML = help;
		}
		/* // 第一种修改方式
			function makeHelpCallback(help) {
			  return function() {
				showHelp(help);
			  };
			}
		*/
		function setupHelp() {
		  var helpText = [
			  {'id': 'email', 'help': 'Your e-mail address'},
			  {'id': 'name', 'help': 'Your full name'},
			  {'id': 'age', 'help': 'Your age (you must be over 16) this.$message.error($1)'}
			];

		  for (var i = 0; i < helpText.length; i++) {
				var item = helpText[i]; // 第三种修改方式 let item = helpText[i];
				document.getElementById(item.id).onfocus = function() {
				  showHelp(item.help);
				}
				/* // 第一种修改方式 
					document.getElementById(item.id).onfocus = makeHelpCallback(item.help);
				*/
				/* //第二种修改方式
					(function() {
					   var item = helpText[i];
					   document.getElementById(item.id).onfocus = function() {
						 showHelp(item.help);
					   }
					})(); // 马上把当前循环项的item与事件回调相关联起来
				*/
				/* //第四种方式
					helpText.forEach(function(text) {
						document.getElementById(text.id).onfocus = function() {
						  showHelp(text.help);
						}
					  });
				*/
		  }
		}
		setupHelp(); 
	</script>
</html>
```

******

## 四、javascript 中将两个变量互换，你能想到几种方式？
```js
var a = 5;
var b = 10;

a 和 b 变量互换


第一种是建中间变量
第二种是 异或 a = a^b; b=a^b; a=a^b
第三种 a = a + b; b = a - b; a = a - b;
第四种 console.log("a="+b);console.log("b="+a); 
第五种 a=[b，b=a][0];
第六种 var [a,b] = [b,a]
```

******

## 五、在浏览器中 从输入地址到最终的页面渲染完成 发生了什么，经过哪几部分？

1. DNS 域名解析，把域名解析成 ip 地址；
2. 通过 tcp 协议与服务器握手跟服务器建立链接；
3. 浏览器向服务器发送请求；
4. 服务器响应，若状态码为 200 浏览器接受返回的HTML页面开始渲染；
5. 浏览器深度遍历 HTML 节点生成 dom 树；
6. 解析 css dom 树并应用他们；
7. js 根据新的渲染树计算各个节点的位置。

******

## 六、前端如何做优化
了解了浏览器渲染界面的过程其实对于前端的优化就有思路了
1. css 放在文件头部，javascript 放在底部；
2. 尽量减少 http 请求次数；
3. 善用缓存不要重复加载相同的资源，比如用户登录之后的用户信息等；
4. 图片优化，采用图片懒加载，在页面开始加载的时候，不请求真实图片地址，而是用默认图占位，当前页面加载完成后，在根据相关的条件依次加载真实图片；
5. 降低css选择器的复杂性，尽量使用 id 和 class。
以上只是几种比较典型的优化方式，除了这些还有很多细节的优化。

******

## 七、Sql脚本注入原理是什么？如何防止脚本注入？
主要原理：
通过把SQL命令插入到Web表单提交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令
防止：
1. 使用预编译，绑定变量，避免出现执行拼接字符的情况；
2. 过滤特殊字符和语句；
3. 页面不错误回显。

******

## 八、position的absolute与fixed共同点与不同点是什么？
共同点：
1. 改变行内元素的呈现方式，display被置为inline-block；
2. 让元素脱离普通流，不占据空间；
3. 默认会覆盖到非定位元素上

不同点：
absolute的”根元素“是可以设置的，而fixed的”根元素“固定为浏览器窗口；
当你滚动网页，fixed元素与浏览器窗口之间的距离是不变的。

******

## 九、编写一个函数来计算第 N 个斐波那契数
```js
function fibonacci(n){
    if (typeof n !== "number" || n < 1) {
	return n;
    }else if (n <= 2) {
	return 1;
    }else{
	return fibonacci(n-1) + fibonacci(n-2);
    }  
}
var temp = fibonacci(8)
console.log(temp)
```
> 注意：执行此函数时数字不宜过大(超过100电脑会有卡顿，超过1000电脑会有明显卡顿，超过10000电脑会卡死)

******

## 十、display:none和visibility:hidden的区别？
display:none  隐藏对应的元素，在文档布局中不再给它分配空间，它各边的元素会合拢，
就当他从来不存在。
visibility:hidden  隐藏对应的元素，但是在文档布局中仍保留原来的空间。

******

## 十一、什么是回流和重绘？回流和重绘的区别是什么？
当render tree中的一部分(或全部)因为元素的规模尺寸、布局、隐藏等改变而需要重新构建，这就称为回流(reflow)。
当render tree中的一些元素需要更新属性，而这些属性只是影响元素的外观，风格，而不会影响布局的，比如background-color。则就叫称为重绘。
回流必将引起重绘，而重绘不一定会引起回流。

******

## 十二、你如何向非技术背景的利益相关者解释 API 的概念？
API 是不同软件产品之间的通信使者。它让各个软件系统之间可以相互通信和同步。
例如，你可以使用 Facebook 的 API 在你自己的网站上显示你在 Facebook 发布的帖子，并允许人们直接在你的网站上共享或评论你的帖子，无需切换到 Facebook 上。

******

## 十三、Ajax 是什么？Ajax 的交互模型？同步和异步的区别？如何解决跨域问题？

Ajax 是什么
1. 通过异步模式，提升了用户体验
2. 优化了浏览器和服务器之间的传输，减少不必要的数据往返，减少了带宽占用
3. Ajax 在客户端运行，承担了一部分本来由服务器承担的工作，减少了大用户量下的服务器负载。

Ajax 的最大的特点
1. Ajax可以实现动态刷新(局部刷新)
2. readyState 属性 状态 有5个可取值： 0 = 未初始化，1 = 启动， 2 = 发送，3 = 接收，4 = 完成

Ajax 同步和异步的区别
1. 同步：提交请求 -> 等待服务器处理 -> 处理完毕返回，这个期间客户端浏览器不能干任何事
2. 异步：请求通过事件触发 -> 服务器处理(这时浏览器仍然可以作其他事情)-> 处理完毕

> 备注：ajax.open方法中，第3个参数是设同步或者异步。

解决跨域问题
1. jsonp
2. iframe
3. window.name、window.postMessage
4. 服务器上设置代理页面


附：

Ajax 的缺点
1. Ajax 不支持浏览器 back 按钮
2. 安全问题 Ajax 暴露了与服务器交互的细节
3. 对搜索引擎的支持比较弱
4. 破坏了程序的异常机制
5. 不容易调试

******

## 十四、CSS 选择符有哪些？哪些属性可以继承？优先级算法如何计算？

CSS 选择符：
1. id选择器(# myid)
2. 类选择器(.myclassname)
3. 标签选择器(div, h1, p)
4. 相邻选择器(h1 + p)
5. 子选择器(ul > li)
6. 后代选择器(li a)
7. 通配符选择器( * )
8. 属性选择器(a[rel = "external"])
9. 伪类选择器(a: hover, li:nth-child)

可继承的属性：
1. font-size
2. font-family
3. color
4. text-indent

优先级算法：
1. 优先级就近原则，同权重情况下样式定义最近者为准;
2. 载入样式以最后载入的定位为准;
3. !important >  id > class > tag  
4. important 比 内联优先级高，但内联比 id 要高

附：
不可继承属性：
1. border
2. padding
3. margin
4. width
5. eight

******

## 十五、display 值的作用和 position 取值的区别？

display 值的作用：
1. block 像块类型元素一样显示。
2. inline 缺省值。像行内元素类型一样显示。
3. inline-block 像行内元素一样显示，但其内容像块类型元素一样显示。
4. list-item 像块类型元素一样显示，并添加样式列表标记。

position 值的定位区别：
1. absolute 生成绝对定位的元素，相对于 static 定位以外的第一个祖先元素进行定位。
2. fixed 生成固定定位的元素，相对于浏览器窗口进行定位（老IE不支持）。
3. relative 生成相对定位的元素，相对于其在普通流中的位置进行定位。
4. static 默认值。没有定位，元素出现在正常的流中。
5. inherit 规定从父元素继承 position 属性的值。

******

## 十六、null和undefined的区别是什么？

null 表示没有对象，转化为数值时为 0
undefined 表示缺少值，转化为数值时为 NaN

undefined 典型用法：
1. 变量被声明了，但没有赋值时，就等于 undefined
2. 调用函数时，应该提供的参数没有提供，该参数等于 undefined
3. 对象没有赋值的属性，该属性的值为 undefined
4. 函数没有返回值时，默认返回 undefined

null 典型用法：
1. 作为函数的参数，表示该函数的参数不是对象
2. 作为对象原型链的终点

******

## 十七、线程与进程的区别是什么？

1. 一个程序至少有一个进程,一个进程至少有一个线程；
2. 线程的划分尺度小于进程，使得多线程程序的并发性高；
3. 进程在执行过程中拥有独立的内存单元，而多个线程共享内存，从而极大地提高了程序的运行效率；
4. 每个独立的线程有一个程序运行的入口、顺序执行序列和程序的出口。但是线程不能够独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制；
5. 从逻辑角度来看，多线程的意义在于一个应用程序中，有多个执行部分可以同时执行。但操作系统并没有将多个线程看作多个独立的应用，来实现进程的调度和管理以及资源分配。

******

## 十八、什么是 HTTP？ HTTP 与 HTTPS 有什么区别？他们的特点是什么？

HTTP（HyperText Transfer Protocol：超文本传输协议）是一种用于分布式、协作式和超媒体信息系统的应用层协议。 
简单来说就是一种发布和接收 HTML 页面的方法，被用于在 Web 浏览器和网站服务器之间传递信息。

HTTPS和HTTP的区别：
1. https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。
2. http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。
3. http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。
4. http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全

HTTP特点：
1. 无状态：协议对客户端没有状态存储，对事物处理没有“记忆”能力，比如访问一个网站需要反复进行登录操作
2. 无连接：HTTP/1.1之前，由于无状态特点，每次请求需要通过TCP三次握手四次挥手，和服务器重新建立连接。比如某个客户机在短时间多次请求同一个资源，服务器并不能区别是否已经响应过用户的请求，所以每次需要重新响应请求，需要耗费不必要的时间和流量。
3. 基于请求和响应：基本的特性，由客户端发起请求，服务端响应
4. 简单快速、灵活
5. 通信使用明文、请求和响应不会对通信方进行确认、无法保护数据的完整性

HTTPS有如下特点：
1. 内容加密：采用混合加密技术，中间者无法直接查看明文内容
2. 验证身份：通过证书认证客户端访问的是自己的服务器
3. 保护数据完整性：防止传输的内容被中间人冒充或者篡改

******

## 十九、HTTP 四个常用请求方法是什么？功能分别是什么？

四个常用请求方法：GET、POST、PUT、DELETE

功能：
GET: 请求指定的页面信息，并返回实体主体。
POST: 向指定资源提交数据进行处理请求,数据被包含在请求体中;POST 请求可能会导致新的资源的建立或已有资源的修改。
PUT: 从客户端向服务器传送的数据取代指定的文档的内容。
DELETE: 请求服务器删除指定的页面。

******

## 二十、常用的你们所能想到的 HTTP 状态码有哪些？

100 Continue  继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息
200 OK   正常返回信息
201 Created  请求成功并且服务器创建了新的资源
202 Accepted  服务器已接受请求，但尚未处理
301 Moved Permanently  请求的网页已永久移动到新位置
302 Found  临时性重定向
303 See Other  临时性重定向，且总是使用 GET 请求新的 URI
304 Not Modified  自从上次请求后，请求的网页未修改过
400 Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求
401 Unauthorized  请求未授权
403 Forbidden  禁止访问
404 Not Found  找不到如何与 URI 相匹配的资源
500 Internal Server Error  最常见的服务器端错误
503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）

******

## 二十一、遍历数组的方式有哪些？

1. 普通 for 循环
2. foreach 循环
3. for in 循环
4. map 遍历
5. for of 遍历

******

## 二十二、大家知道 Node.js 有哪些使用场景吗？

1. 高并发
2. 聊天
3. 实时消息推送   

******

## 二十三、Node的优点和缺点是什么？

优点：
1. 因为Node是基于事件驱动和无阻塞的，所以非常适合处理并发请求，因此构建在Node上的代理服务器相比其他技术实现的服务器表现要好得多。
2. 与Node代理服务器交互的客户端代码是由javascript语言编写的，因此客户端和服务器端都用同一种语言编写，这是非常美妙的事情。

缺点：
1. Node是一个相对新的开源项目，所以不太稳定，它总是一直在变。
2. 缺少足够多的第三方库支持。

******

## 二十四、CSS3有哪些新特性？

1. CSS3实现圆角（border-radius），阴影（box-shadow），
2. 对文字加特效（text-shadow、），线性渐变（gradient），旋转（transform）
3. transform:rotate(9deg) scale(0.85,0.90) translate(0px,-30px) skew(-9deg,0deg);// 旋转,缩放,定位,倾斜
4. 增加了更多的CSS选择器  多背景 rgba
5. 在CSS3中唯一引入的伪类是 ::selection.
6. 媒体查询，多栏布局
7. border-image

******

## 二十五、谈谈对 BFC 规范的理解？

BFC 是 W3C CSS 2.1 规范中的一个概念，它决定了元素如何对其内容进行布局，以及与其他元素的关系和相互作用。
BFC，块级格式化上下文，一个创建了新的BFC的盒子是独立布局的，盒子里面的子元素的样式不会影响到外面的元素。
在同一个 BFC 中的两个毗邻的块级盒在垂直方向（和布局方向有关系）的 margin 会发生折叠。

******

## 二十六、CSS 中 link 和 @import 有什么区别？

1. link属于XHTML标签，而 @import 是 CSS 提供的，只能加载CSS;
2. link引用CSS时，在页面载入时同时加载，而 @import 需要页面网页完全载入以后加载;
3. link是XHTML标签，无兼容问题；@import 是在 CSS2.1 提出的，低版本的浏览器不支持。

******

## 二十七、HTML 与 XHTML 的区别是什么？

html: 超文本标记语言 (Hyper Text Markup Language)
xhtml： xhtml中的 x 是 EXtensible 的表示，XHTML 指可扩展超文本标记语言（EXtensible HyperText Markup Language），是一种置标语言，表现方式与超文本标记语言（HTML）类似，不过语法上更加严格更纯净。

区别：
1. XHTML 元素必须被正确地嵌套。
2. XHTML 元素必须被关闭。
3. 标签名必须用小写字母。
4. XHTML 文档必须拥有根元素。

******

## 二十八、CSS sprites 是什么？如何在页面或网站中使用它？

CSS Sprites 是一种网页图片应用处理方式，在国内很多人叫 css 精灵。
CSSSprites把网页中一些背景图片整合到一张图片文件中，再利用 CSS 的"background-image"，"background-repeat"，"background-position" 的组合进行背景定位，background-position 可以用数字能精确的定位出背景图片的位置。
这样可以减少很多图片请求的开销，因为请求耗时比较长；请求虽然可以并发，但是也有限制，一般浏览器都是6个。
对于当前网络流行的速度而言，不高于200KB的单张图片的所需载入时间基本是差不多的，所以无需顾忌这个问题。

******

## 二十九、HTML 语义化

HTML 根据内容的语义化，选择合适的标签便于开发者阅读和写出更优雅的代码的同时让浏览器的爬虫和机器很好地解析。
语义化基本上都是围绕着几个主要的标签，像标题（H1~H6）、列表（li）、强调（strong em）等等

语义化的目的：
1. 去掉或者丢失 CSS 的时候能够让页面呈现出清晰的结构；
2. 有利于SEO：和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息：爬虫依赖于标签来确定上下文和各个关键字的权重；
3. 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以意义的方式来渲染网页；
4. 便于团队开发和维护，语义化使得网页更具可读性，是进一步开发网页的必要步骤，遵循W3C标准的团队都遵循这个标准，可以减少差异化；
5. 提升用户体验，例如title、alt用于解释名词或解释图片信息、label标签的活用。

注意事项：
1. 尽可能少的使用无语义的标签 div 和 span；
2. 在语义不明显时，既可以使用 div 或 p 时，尽量用 p, 因为 p 在默认情况下有上下间距，对兼容特殊终端有利；
3. 不要使用纯样式标签，如：b、font、u等，改用css设置；
4. 需要强调的文本，可以包含在 strong 或 em 标签中（浏览器预设样式，能用CSS指定就不用他们），strong 默认样式是加粗（不要用b），em 是斜体（不用i）；
5. 使用表格时，标题要用 caption，表头用 thead，主体部分用 tbody 包围，尾部用 tfoot 包围。表头和一般单元格要区分开，表头用 th，单元格用 td；
6. 表单域要用 fieldset 标签包起来，并用 legend 标签说明表单的用途；
7. 每个 input 标签对应的说明文本都需要使用 label 标签，并且通过为 input 设置 id 属性，在 lable 标签中设置 for=someld 来让说明文本和相对应的 input 关联起来。

******

## 三十、Doctype作用? 严格模式与混杂模式如何区分？它们有何意义?

Doctype作用：
<!DOCTYPE> 声明位于文档中的最前面，处于 <html> 标签之前。告知浏览器以何种模式来渲染文档。 
严格模式与混杂模式的区分：
严格模式的排版和 JS 运作模式是  以该浏览器支持的最高标准运行。
混杂模式中，页面以宽松的向后兼容的方式显示。模拟老式浏览器的行为以防止站点无法工作。

意义：
DOCTYPE不存在或格式不正确会导致文档以混杂模式呈现。 

******

## 三十一、DOM操作—怎样创建、添加、移除、移动、复制、创建和查找节点
```js
/* 创建新节点： */
createDocumentFragment() // 创建一个DOM片段
createElement() // 创建一个具体的元素
createTextNode() // 创建一个文本节点

/* 添加、移除、替换、插入：*/
appendChild()	// 添加
removeChild()	// 移除
replaceChild()	// 替换
insertBefore()	// 在已有的子节点前插入一个新的子节点

/* 查找：*/
getElementsByTagName() 	// 通过标签名称
getElementsByName() 	// 通过元素的Name属性的值
getElementById() 	// 通过元素Id，唯一性
```

******

## 三十二、iframe 的优点和缺点分别是什么？

优点：
1. 解决加载缓慢的第三方内容如图标和广告等的加载问题
2. Security sandbox
3. 并行加载脚本

缺点：
1. iframe会阻塞主页面的Onload事件
2. 即使内容为空，加载也需要时间
3. 没有语意

******

## 三十三、如何实现浏览器内多个标签页之间的通信?

调用 localstorge、cookies 等本地存储方式

******

## 三十四、什么是 FOUC？如何来避免 FOUC？

FOUC - Flash Of Unstyled Content 文档样式闪烁
```css
<style type="text/css" media="all">@import "../test.css";</style> 
```
造成文档样式闪烁的原因就是引用CSS文件的@import，浏览器会先加载整个HTML文档的DOM，然后再去导入外部的CSS文件，
因此，在页面DOM加载完成到CSS导入完成中间会有一段时间页面上的内容是没有样式的，这段时间的长短跟网速，电脑速度都有关系。
解决方法也比较简单，在 head 之间加入一个 link 或者 script 元素就行了。

******

## 三十五、减少页面加载时间的方法有哪些？

1. 优化图片 
2. 图像格式的选择（GIF：提供的颜色较少，可用在一些对颜色要求不高的地方） 
3. 优化CSS（压缩合并css，如 margin-top, margin-left...) 
4. 网址后加斜杠（如www.baidu.com/目录，会判断这个目录是什么文件类型，或者是目录。） 
5. 标明高度和宽度（如果浏览器没有找到这两个参数，它需要一边下载图片一边计算大小，如果图片很多，浏览器需要不断地调整页面。这不但影响速度，也影响浏览体验。 
当浏览器知道了高度和宽度参数后，即使图片暂时无法显示，页面上也会腾出图片的空位，然后继续加载后面的内容。从而加载时间快了，浏览体验也更好了） 
6. 减少http请求（合并文件，合并图片）

******

## 三十六、什么是内存泄漏？哪些操作会造成内存泄漏？

内存泄漏指任何对象在你不再拥有或需要它之后仍然存在。

造成内存泄漏的操作：
1. setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
2. 闭包
3. 控制台日志
4. 循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）

******

## 三十七、js延迟加载的方式有哪些？

1. defer和async
2. 动态创建DOM方式（创建script，插入到DOM中，加载完毕后callBack）
3. 按需异步载入js

******

## 三十八、前端如何做性能优化？

1. 减少http请求次数：CSS Sprites, JS、CSS 源码压缩、图片大小控制合适；网页 Gzip，CDN 托管，data 缓存 ，图片服务器
2. 前端模板 JS + 数据，减少由于HTML标签导致的带宽浪费，前端用变量保存 AJAX 请求结果，每次操作本地变量，不用请求，减少请求次数
3. 用 innerHTML 代替 DOM 操作，减少 DOM 操作次数，优化 javascript 性能
4. 当需要设置的样式很多时设置 className 而不是直接操作 style
5. 少用全局变量、缓存DOM节点查找的结果。减少 IO 读取操作
6. 避免使用 CSS Expression（css表达式)又称 Dynamic properties(动态属性)
7. 图片预加载，将样式表放在顶部，将脚本放在底部，加上时间戳

******

## 三十九、http状态码有那些？分别代表是什么意思？

1. 100-199 用于指定客户端应响应的某些动作
2. 200-299 用于表示请求成功
3. 300-399 用于已经移动的文件并且常被包含在定位头信息中指定新的地址信息
4. 400-499 用于指出客户端的错误
400：语义有误，当前请求无法被服务器理解
401：当前请求需要用户验证
403：服务器已经理解请求，但是拒绝执行它
5. 500-599 用于指出服务器错误
503：服务不可用
[贵州学致网络科技服务有限公司：常见的HTTP状态码](https://zhuanlan.zhihu.com/p/151386625)

******

## 四十、javascript继承的几种方法

1. 原型链继承
2. 借用构造函数继承
3. 组合继承(原型+借用构造)
4. 原型式继承
5. 寄生式继承
6. 寄生组合式继承

******

## 四十一、ajax 的过程是什么？

1. 创建XMLHttpRequest对象,也就是创建一个异步调用对象
2. 创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息
3. 设置响应HTTP请求状态变化的函数
4. 发送HTTP请求
5. 获取异步调用返回的数据
6. 使用JavaScript和DOM实现局部刷新

******

## 四十二、CSS中 em 与 rem 指的是什么？有什么区别？

rem是CSS3新增的一个相对单位（root em，根em），相对于根元素(即html元素)font-size计算值的倍数
rem（font size of the root element）是指相对于根元素的字体大小的单位。
作用：利用rem可以实现简单的响应式布局，可以利用html元素中字体的大小与屏幕间的比值设置font-size的值实现当屏幕分辨率变化时让元素也变化。
em（font size of the element）是指相对于父元素的字体大小的单位，它与rem之间其实很相似。

区别：
em与rem的重要区别： 它们计算的规则一个是依赖父元素另一个是依赖根元素计算。

******

## 四十三、表单提交中Get和Post方式的区别？

1. get 是从服务器上获取数据， post 是向服务器传送数据。
2. get 是把参数数据队列加到提交表单的 ACTION 属性所指的 URL 中，值和表单内各个字段一一对应，在 URL 中可以看到。 post 是通过 HTTP post 机制，将表单内各个字段与其内容放置在 HTML HEADER 内一起传送到 ACTION 属性所指的 URL 地址 , 用户看不到这个过程。
3. 对于 get 方式，服务器端用 Request.QueryString 获取变量的值，对于 post 方式，服务器端用 Request.Form 获取提交的数据。
4. get 传送的数据量较小，不能大于 2KB 。 post 传送的数据量较大，一般被默认为不受限制。但理论上， IIS4 中最大量为 80KB ， IIS5 中为 100KB 。
5. get 安全性低， post 安全性较高。

******

## 四十四、JavaScript 中的强制转型是指什么？

两种不同的内置类型间的转换被称为强制转型，强制转型在 JavaScript 中有两种形式：显式和隐式。

显式强制转
```js
var a = "99";
var b = Number( a );

//这里 a 是 "99", b 是 99。
//隐式强制转型

var a = "99";
var b = a * 1; // "99" 隐式转型成 99
```

******

## 四十五、JavaScript 中 scope 是指什么？

在 JavaScript 中 scope 是指作用域，每个函数都有自己的作用域。作用域基本上是变量以及如何通过名称访问这些变量的规则的集合。
只有函数中的代码才能访问函数作用域内的变量。
同一个作用域中的变量名必须是唯一的。一个作用域可以嵌套在另一个作用域内。
如果一个作用域嵌套在另一个作用域内，最内部作用域内的代码可以访问另一个作用域的变量。

******

## 四十六、javascript 中严格比较和抽象比较有什么区别？

严格比较( 表示 === )在不允许强制转型的情况下检查两个值是否相等
抽象比较( 表示 == )在允许强制转型的情况下检查两个值是否相等
```js
var a = "66";
var b = 66;
a == b; // true
a === b; // false
```
严格比较不仅比较值还比较值的类型，抽象比较仅比较值，如上所示，会将 a 隐式强制转换成数字再比较。

******

## 四十七、“use strict”的作用是什么？

use strict 出现在 JavaScript 代码的顶部或函数的顶部，可以帮助你写出更安全的 JavaScript 代码。如果你错误地创建了全局变量，它会通过抛出错误的方式来警告你。
```js
function youFun(val) {
 "use strict"; 
 x = val + 10;
}
```
它会抛出一个错误，因为 x 没有被定义，并使用了全局作用域中的某个值对其进行赋值，而 use strict 不允许这样做。修改如下：
```js
function youFun(val) {
 "use strict"; 
 var x = val + 10;
}
```

******

## 四十八、什么是事件冒泡，如何阻止它？

事件冒泡是指嵌套最深的元素触发一个事件，然后这个事件顺着嵌套顺序在父元素上触发。
防止事件冒泡的一种方法是使用 event.cancelBubble、event.stopPropagation()或 event.preventDefault()。

******

## 四十九、什么是 IIFE？

它是立即调用函数表达式（Immediately-Invoked Function Expression），简称 IIFE。函数被创建后立即被执行：
```js
(function IIFE(){
 console.log( "Hello!" );
})();
```

******

## 五十、JavaScript 中“undefined”和“not defined”之间的区别？

在 JavaScript 中，如果你试图使用一个不存在且尚未声明的变量，JavaScript 将抛出错误“var name is not defined”，让后脚本将停止运行。
但如果你使用 typeof 未定义变量，它将返回 undefined。
```js
var a; // a 为声明未定义变量
console.log(b); // b 为未声明也未定义变量
var c = 1; // c 为生命且定义变量
```
所以
```js
console.log(a); // 输出: undefined
console.log(b); // 输出: ReferenceError: b is not defined 	
console.log(c); // 输出 1
```

******

## 五十一、BFC 与 IFC 是什么？如何产生？有何作用？

什么是BFC与IFC
BFC（Block Formatting Context）即“块级格式化上下文”， IFC（Inline Formatting Context）即行内格式化上下文。常规流（也称标准流、普通流）是一个文档在被显示时最常见的布局形态。一个框在常规流中必须属于一个格式化上下文，你可以把BFC想象成一个大箱子，箱子外边的元素将不与箱子内的元素产生作用。
BFC是W3C CSS 2.1 规范中的一个概念，它决定了元素如何对其内容进行定位，以及与其他元素的关系和相互作用。当涉及到可视化布局的时候，Block Formatting Context提供了一个环境，HTML元素在这个环境中按照一定规则进行布局。一个环境中的元素不会影响到其它环境中的布局。比如浮动元素会形成BFC，浮动元素内部子元素的主要受该浮动元素影响，两个浮动元素之间是互不影响的。也可以说BFC就是一个作用范围。
在普通流中的 Box(框) 属于一种 formatting context(格式化上下文) ，类型可以是 block ，或者是 inline ，但不能同时属于这两者。并且， Block boxes(块框) 在 block formatting context(块格式化上下文) 里格式化， Inline boxes(块内框) 则在 Inline Formatting Context(行内格式化上下文) 里格式化。
如何产生BFC
当一个HTML元素满足下面条件的任何一点，都可以产生Block Formatting Context：
float的值不为none
overflow的值不为visible
display的值为table-cell, table-caption, inline-block中的任何一个
position的值不为relative和static
CSS3触发BFC方式则可以简单描述为：在元素定位非static，relative的情况下触发，float也是一种定位方式。

BFC的作用与特点
a、不和浮动元素重叠，清除外部浮动，阻止浮动元素覆盖
如果一个浮动元素后面跟着一个非浮动的元素，那么就会产生一个重叠的现象。常规流（也称标准流、普通流）是一个文档在被显示时最常见的布局形态，当float不为none时，position为absolute、fixed时元素将脱离标准流。
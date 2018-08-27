<!-- TOC -->

- [标准模式与兼容模式（怪异模式）](#标准模式与兼容模式怪异模式)
- [Doctype含义、作用](#doctype含义作用)
- [渐进增强和优雅降级](#渐进增强和优雅降级)
- [简述src与href的区别](#简述src与href的区别)
- [如何理解HTML的语义化](#如何理解html的语义化)
- [HTML5新增了哪些内容或API，如何使用](#html5新增了哪些内容或api如何使用)
    - [新元素(包括标签、属性)](#新元素包括标签属性)
    - [API](#api)
    - [标签](#标签)
- [meta](#meta)
- [viewpoint](#viewpoint)
- [如何处理移动端1px被处理成2px的问题](#如何处理移动端1px被处理成2px的问题)
- [audio元素和video元素在iOS和Android中无法自动播放](#audio元素和video元素在ios和android中无法自动播放)
- [HTML和XHTML区别](#html和xhtml区别)
- [移动端布局方式，rem布局原理（width=device-width）](#移动端布局方式rem布局原理widthdevice-width)

<!-- /TOC -->


### 标准模式与兼容模式（怪异模式）
在很久以前的网络上，页面通常有两种版本：
- 为网景（Netscape）的 Navigator 准备的版本；
- 为微软（Microsoft）的Internet Explorer准备的版本。

当W3C创立网络标准后，为了不破坏当前既有的网站，浏览器不能直接起用这些标准。因此，浏览器采用了两种模式，用以把能符合新规范的网站和老旧网站区分开。

目前浏览器的排版引擎有三种模式：怪异模式（Quirks mode）、接近标准模式（Almost standards mode）和标准模式（Standards mode）。

在怪异模式下，排版会模拟Navigator 4 与 Internet Explorer 5 的非标准行为。为了支持在网络标准被广泛采用前就已经建好的网站，这么做是必要的；

在标准模式下，行为即（但愿如此）由HTML于CSS的规范描述的行为；

在接近标准模式下，只有少数的怪异行为被实现。

其具体的差别有：
- IE的盒模型的处理差异：标准CSS盒模型的宽度和高度等于内容区的高度和宽度，不包含内边距和边框；而IE6之前的浏览器实现的盒模型的宽高计算方式是包含内边距和边框的；
- 行内元素的垂直对其：很多早期的浏览器对齐图片至包含它们的盒子的下边框，虽然CSS的规范要求它们被对齐至盒内文本的基线；标准模式下，基于Gecko的浏览器将会对齐至基线（在标准模式下，图片并不是与父元素的下边框对齐的、而是基线对齐的，仔细观察，会发现图片与父元素下边框之间存在一点小空隙），而在quirks模式下它们会对齐至底部。


### Doctype含义、作用
用于告诉浏览器，用怪异模式还是标准模式处理HTML文件。

标准模式使用：`<!DOCTYPE html>`。这是所有可用的DOCTYPE之中最简单的、而且是HTML5推荐的。使用其他更复杂的DOCTYPE并没有什么好处、而且可能会让你冒着触发接近标准模式或者怪异模式的风险。

请正确地把DOCTYPE放在HTML文件顶端，任何放在DOCTYPE前端的东西，比如注释或XML声明，会令Internet Explorer 9 或更早期的浏览器触发怪异模式。

在HTML5中，DOCTYPE唯一的作用是启动标准模式。更早期的HTML标准会附加其他意义，但没有任何浏览器会将DOCTYPE用于怪异模式和标准模式之间互换以外的用途。


### 渐进增强和优雅降级

渐进增强和优雅降级这两个概念实在CSS3出现之后火起来的。由于低级浏览器不支持CSS3，但是CSS3特效太优秀不忍放弃，所以产生了一种解决方式：在高级浏览器中使用CSS3，而在低级浏览器中只保证最基本的功能。

渐进增强（Progressive Enhancement）：一开始就针对低版本浏览器进行构建页面，完成基本的功能，然后再针对高级浏览器进行效果、交互、追加功能达到更好的体验。即在不影响老浏览器的正常显示与使用的前提下来增强体验；

优雅降级(Graceful Degradation)：一开始就构建站点的完整功能，然后针对浏览器测试和修复。比如一开始使用CSS3的特性构建了一个应用，然后逐步针对各大浏览器进行hack使其可以在低版本浏览器上正常浏览。

举个例子：
```css
.transition { /* 渐进增强 */
	-webkit-transition: all .5s;
	-moz-transition: all .5s;
	-o-transition: all .5s;
	transition: all .5s;
}
.transition{ /* 优雅降级 */
	transition: all .5s;
	-o-transition: all .5s;
	-moz-transition: all .5s;
	-webkit-transition: all .5s;
}
```
假设写一个表单，用一个a标签的click事件做提交，如果JavaScript被禁用了、则表单提交功能也失效了。假如使用`<input type="submit" />`，那么即使JavaScript被禁用、依然可以提交。
所以和渐进增强相比，优雅降级还有其特别的意义。优雅降级需要正确地体现HTML标签的语义（渐进增强一般说的是CSS3技术），符合“浏览器的预期”，让你的网页在各种情况下（包括JavaScript被禁用、css传输失败等降级情况）的情形都可以运作良好。

那么实际开发过程中选渐进增强还是优雅降级呢？如果**低版本**用户居多，则优先采用**渐进增强**；反之如果**高版本**用户居多，为了提高大多数用户的使用体验，优先采用**优雅降级**的开发流程。现在的大多数大公司都是采用渐进增强的方式，因为业务优先，要优先保障产品对绝大多数用户的可用性，再去渐进增强，采用新功能给高版本用户提供更好的用户体验。

### 简述src与href的区别
- src：source（源），用于替换当前元素，src指向的内容会嵌入到文档中当前标签所在的位置，比如img、script、iframe、style。举个例子：
`<script src="script.js"></script>`
当浏览器解析到该元素时，会暂停浏览器的渲染，直到该资源加载完毕。这也是将js脚本放在页面底部而不是头部的原因。

- href：Hypertext Reference（超文本引用），用来建立当前元素和文档之间的连接，比如link、a。举个例子：
`<link href="reset.css" ref="stylesheet" />`
浏览器会识别该文档为css文档，并行下载该文档，并且不会停止对当前文档的处理。这也是建议使用link，而不是采用@import加载css的原因。

### 如何理解HTML的语义化
1. 什么是语义化
语义化是指用合适的标签（代码语义化）来呈现结构化的内容（内容语义化），以便让机器更好地读懂内容，同时更有利于提升其可读性和可维护性。

2. 为什么语义化
	- 有利于SEO，有助于爬虫抓取更多的有效信息，爬虫依赖于标签来确定上下文和各个关键字的权重；
	- 语义化的HTML在没有CSS的情况下也能呈现较好的内容结构和代码结构；
	- 方便其他设备解析（如听障设备、移动设备）；
	- 便于团队开发和维护。

3. 有哪些语义化标签
	- 文档章节类HTML标签
		- article / section：article无论从结构还是内容上来说，都是独立的、完整的，当一段内容脱离了所在的语境后还是完整的、独立的，那就应该用article标签；section应用的典型场景有文章的章节、标签对话框中的标签页或者论文中有编号的部分。对于一段主题性的内容，可以使用section；假如这段内容可以脱离上下文、作为完整的独立存在，那就使用article。原则上来说，能使用article的时候，也是可以使用section的，但是实际上，假入article更合适，那就不要使用section。
		- header / footer：网页或文章的页眉、页尾。header作为整个页面或者一个内容块的头部部分，有可以包裹一节的目录部分；footer代表网页或一个内容块的底部部分，通常含有该节的一些基本信息。它们都没有个数限制。
		- hgroup：如果有连续多个h1-h6标签，可以使用hgroup包裹。比如有一个章节的头部信息包括连续多个h1-h6标签以及其他文章数据，那么h1-h6标签就用hgroup包住，连同其他文章数据一起放入header标签中。
		- h1 - h6：表示标题，权重逐渐降低。一个页面允许出现多个（包括h1）。
		- main：整个页面的主题部分，一个页面只能有一个main（HTML 5.2将可以使用多个main），不能放进其他独立单元里，它包括一个页面最核心的内容。
		- aside：一般表示文章主体内容以外的附属信息，其中内容可以是与当前文章有关的相关资料、标签、名词解释等，最典型的是作为侧边栏；也可以表示一些工具功能，如“分享文章”、“回到顶部”等功能。
		- nav：表示网站的导航，但不一定所有的导航都需要用nav标签，建议仅用来实现比较重要的导航，如网页头部导航，如网页页脚的链接导航列表用footer即可。每个页面可以有多个nav。
	- 文本类HTML标签
		- a：表示一个通向其他页面或当前页面其他位置的入口，这是一个历史悠久的语义化标签，同时也是搜索引擎的基础。
		- p：表示一个段落。
		- em / strong：用来强调某个词或某个句子。从语气上来说，strong比em强调意味更重。
		- time：用来表示24小时制时间或公历日期，若表示日期则可以包含时间和时区。对于time，尽量用机器能识别的时间格式，而不要用一些模糊的表达，如“一小时前”、“两天前”等。
		- address：表示区块容器，必须是作为联系信息出现，比如邮编地址、邮件地址等等，一般出现在footer。
	- 组合型HTML标签
		- figure：表示一段富文本，通常搭配figcaption来描述这段富文本的描述/标题，一个figure下只能有一个figcaption。

4. 编写HTML语义化代码注意事项
	- 尽量使用语义化标签，而不是使用无语义的div和span堆砌；
	- 在语义不明显时，既可以使用div也可以使用p时，尽量用p，因为p在默认情况下有上下间距，对兼容特殊终端有利；
	- 尽量不要使用纯样式标签如b、font、u等，样式用css设置；
	- 需要强调的文本，使用strong或者em标签（前者默认样式是加粗、后者默认样式是斜体），而不是使用b或i标签；
	- 使用表格时，标题用caption，表头用thead，主体部分用tbody，尾部用tfootb；表头和一般单元格要区分开，表头用th，一般单元格用td；
	- 表单域用fieldset，并用legend标签说明表单的用途；
	- 每个input标签对应的说明文本都需要使用label标签，并通过为input设置id属性，在label标签中设置for=someId来让说明文本和相对应的input关联起来。

参考文章：
- https://www.cnblogs.com/fliu/articles/5244866.html
- https://juejin.im/post/5a9c8866f265da23741072bf

### HTML5新增了哪些内容或API，如何使用
#### 新元素(包括标签、属性)
[w3school](http://www.w3school.com.cn/html/html5_new_elements.asp)。

#### API
1. Canvas
Canvas本质上是一个位图画布。
> 使用canvas编程，首先要获取其上下文（context）；接着在上下文中执行动作；最后将这些动作应用到上下文中。可以将canvas的这种编程方式想象成数据库事务：开发人员先发起一个事务，然后执行某些操作，最后提交事务。

举个例子：
```html
<canvas id="diagonal" style="border:1px solid;" width="200" height="200"></canvas>
```
```javascript
function drawDiagnoal () {
	// 取canvas元素及其绘图上下文
	var canvas = document.getElementbyId('diagonal');
	var context = canvas.getContext('2d');
	
	// 用绝对坐标来创建一条路径
	context.beginPath();
	context.moveTo(70, 140);
	context.lineTo(140, 70);
	
	// 将这条线绘制到canvas上
	context.stroke();
}
window.addEventListener('load', drawDiagnoal, true);
```

2. Audio和Video
HTML5中的多媒体支持。
```html
<audio id="clickSound">
	<srource src="a.ogg" type="audio/ogg">
	<srource src="b.mp3" type="audio/mped">
	您的浏览器不支持audio元素
</audio>

<button id="toggle" onclick="toggleSound()">Play</button>
```
```javascript
function toggleSound () {
	var music = document.getElementById('clickSound');
	var toggle = document.getElementById('toggle');
	if (music.paused) {
		music.play();
		toggle.innerHTML = 'Pause';
	} else {
		music.pause();
		toggle.innerHTML = 'Play';
	}
}
```

```html
<video width="320" height="240" controls>
	<source src="movie.mp4" type="video/mp4">
	<source src="movie.ogg" type="video/ogg">
	您的浏览器不支持 video 标签。
</video>
```

3. Geolocation
> 请求一个位置信息，如果用户同意，浏览器就会返回位置信息，改位置信息是通过支持HTML5地理定位功能的底层设备提供给浏览器的。位置信息由经度/维度坐标和一些其他的元数据组成。有了这些位置信息就可以构建位置感知类应用程序。

两种类型的定位请求API：单次定位请求和重复性的位置更新请求；单次定位（获取当前定位）：getCurrentPosition()；重复新的位置更新（监视定位，设备发生移动或获取到了更高精度的地理位置信息）：
```javascript
function loadDemo () {
	if (navigator.geolocation) {
		navigator.geolocation.watchPosition(
			updateLocation,
			handleLocationError,
			{ maximumAge: 20000 }
		);
	}
}

function updateLocation (position) {
	var latitude = position.coords.latitude,
		longitude = position.coords.longitude,
		accuracy = position.coords.accuracy,
		timestamp = position.timestamp;
}

window.addEventListener('load', loadDemo, true);
```
4. WebSocket
> WebSocket作为HTML5一种新的协议，实现了浏览器与服务器的双向通讯（双工通信，full-duplex）。在WebScoket API中，浏览器和服务器只需要做一个握手的动作，浏览器和服务器之间就形成了一条快速通道，两者就可以直接进行数据传送。

在WebSocket协议中，为什么实现即时服务带来了两大好处：
- Header：互相沟通的Header是很小的，大概只有2 Bytes；
- Server Push

	```javascript
	var wsServer = 'ws://localhost:8888/Demo';
	var websocket = new WebSocket(wsServer);

	websocket.onopen = function (evt) { onOpen(evt) };
	websocket.onclose = function (evt) { onClose(evt) };
	websocket.onmessage = function (evt) { onMessage(evt) };
	websocket.onerror = function (evt) { onError(evt) };

	function onOpen(evt) {
		console.log("Connected to WebSocket server.");
	}
	function onClose(evt) {
		console.log("Disconnected");
	}
	function onMessage(evt) {
		console.log('Retrieved data from server: ' + evt.data);
	}
	function onError(evt) {
		console.log('Error occured: ' + evt.data);
	}
	```
	在实现websocket连线过程中，需要通过浏览器发出websocket连线请求，然后服务器发出回应，这个过程通常称为“握手”（handshaking）。
	作为这一设计原则的一部分，WebSocket链接的协议规范定义了一个HTTP链接作为其开始生命周期，进而保证其与pre-WebSocket世界的完全向后兼容。通常来说HTTP协议切换WebSocket称为WebSocket握手。

5. Web storage
	- localStorage和sessionStorage；
	- 离线缓存

6. Communication
7. Web Workers
8. requestAnimationFrame
> 浏览器可以优化并行的动画动作，更合理地重新排列动作序列，并把能够合并的动作放在一个渲染周期内完成，从而呈现出更流畅的动画效果。比如，通过requestAnimationFrame()，JS动画能够和CSS动画/变换或SVG SMIL动画同步发生。另外，如果在一个浏览器标签页里运行一个动画，当这个标签页不可见时，浏览器会暂停它，这回减少CPU内存的压力，节省电池电量。

9. document.querySelector()、document.querySelectorAll()
`document.querySelector()`根据css选择器返回第一个匹配的元素，如果没有匹配返回null；`document.querySelectorAll()`和`document.querySelector()`作用一样的，只是前者返回的是元素数组，如果`querySelectorAll`没有匹配的内容返回的是一个空数组。

10. classList：元素的所有class name的数组
	```html
	<body>  
		<ul class="class1 class2 class3 ">  
			<li>1</li>  
			<li>2</li>  
			<li>3</li>  
			<li>4</li>  
		</ul>  
		<script>  
			var ul = document.getElementsByTagName("ul")[0];  
			console.log(ul.classList.item(0)); // class1
			ul.classList.add("class4"); // 此时ul的class为：class1 class2 class3 class4
			ul.classList.remove("class4"); // 此时ul的class为：class1 class2 class3
			console.log(ul.classList.contains("class1")); // true
		</script>  
	</body>  
	```

11. 全屏：FullScreen
```javascript
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>Document</title>
  <style>
    html:-moz-full-screen {
      background: red;
    }
    html:-webkit-full-screen {
      background: red;
    }
    html:fullscreen {
      background: red;
    }
  </style>
</head>
<body>
<ul class="class1 class2 class3 ">
  <li onclick="launchFullScreen()">全屏</li>
  <input type="text">
</ul>
<button onclick="exitFullscreen()">click me</button>
<script>
  // 找到支持的方法, 使用需要全屏的 element 调用
  function launchFullScreen(element) {
    var element=element||document.documentElement;
    alert(element.nodeName);
    if (element.requestFullscreen) {
      element.requestFullscreen();
    } else if (element.mozRequestFullScreen) {
      element.mozRequestFullScreen();
    } else if (element.webkitRequestFullscreen) {
      element.webkitRequestFullscreen();
    } else if (element.msRequestFullscreen) {
      element.msRequestFullscreen();
    }
  }
 
  //请注意: exitFullscreen 只能通过 document 对象调用 —— 而不是使用普通的 DOM element.
  function exitFullscreen() {
    if (document.exitFullscreen) {
      document.exitFullscreen();
    } else if (document.mozExitFullScreen) {
      document.mozExitFullScreen();
    } else if (document.webkitExitFullscreen) {
      document.webkitExitFullscreen();
    }
  }
 
  element.webkitRequestFullScreen(Element.ALLOW_KEYBOARD_INPUT);//全屏状态允许键盘输入
 
  /*有的时候为了用户友好体验，在进入全屏或者退出全屏的时候，需要给用户提示，
  这个时候我们可以使用FullScreen的screenchange事件进行监控。*/
  document.addEventListener("fullscreenchange", function () {
   fullscreenState.innerHTML = (document.fullscreen)? "" : "not ";
  }, false);
 
  document.addEventListener("mozfullscreenchange", function () {
    fullscreenState.innerHTML = (document.mozFullScreen)? "" : "not ";
  }, false);
 
  document.addEventListener("webkitfullscreenchange", function () {
    fullscreenState.innerHTML = (document.webkitIsFullScreen)? "" : "not ";
  }, false);
</script>
</body>
</html>
```

12. 页面可见性(Page visibility)
所谓页面可预见性就是当前页面是处于显示状态还是隐藏状态。
页面可预见性对于网站统计非常有用，其具体应用有：统计用户停留在每个页面的时间——用户打开网页到网页关闭或者最小化的时间，即页面可见的时间；控制视频播放页面只在可见时定期刷新——当用户离开视频播放器而使播放页面自动暂停时，控制定期刷新内容的页面不进行刷新；等等。
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<ul class="class1 class2 class3 ">
  <li>1</li>
  <li>2</li>
  <li>3</li>
  <li>4</li>
</ul>
<script>
  (function () {
    var hidden = "hidden";
    // Standards:
    if (hidden in document)
      document.addEventListener("visibilitychange", onchange);
    else if ((hidden = "mozHidden") in document)
      document.addEventListener("mozvisibilitychange", onchange);
    else if ((hidden = "webkitHidden") in document)
      document.addEventListener("webkitvisibilitychange", onchange);
    else if ((hidden = "msHidden") in document)
      document.addEventListener("msvisibilitychange", onchange);
    // IE 9 and lower:
    else if ("onfocusin" in document)
      document.onfocusin = document.onfocusout = onchange;
    // All others:
    else
      window.onpageshow = window.onpagehide
        = window.onfocus = window.onblur = onchange;
    function onchange(evt) {
      var v = "visible", h = "hidden",
        evtMap = {
          focus: v, focusin: v, pageshow: v, blur: h, focusout: h, pagehide: h
        };
      evt = evt || window.event;
      if (evt.type in evtMap)
        document.body.className = evtMap[evt.type];
      else
        document.body.className = this[hidden] ? "hidden" : "visible";
    }
 
    // set the initial state (but only if browser supports the Page Visibility API)
    if (document[hidden] !== undefined)
      onchange({type: document[hidden] ? "blur" : "focus"});
  })();
</script>
</body>
</html>
```

13.  预加载

参考文章：
- http://jartto.wang/2016/07/25/make-an-inventory-of-html5-api/

#### 标签
- HTML5新增的标签：
	- 图形绘制：canvas
	- 新多媒体元素：audio、video、source（定义多媒体资源，audio或video）、embed（定义嵌入的内容，如插件）、track（为如video和audio元素之类的媒介规定外部文本轨道）
	- 新表单元素：datalist（定义选项列表，请与input元素配合使用、来定义input可能的值）、keygen（规定用于表单的密钥对生成器字段）、output（定义不同类型的输出，如脚本的输出）
	- 新的语义和结构元素：article、section、aside、nav、footer、header、bdi、command、details、dialog、summary、figure、figcaption、mark、meter、progress、ruby、rt、rp、time、wbr
- HTML5移除的标签：
	- 纯表现的元素：acronym（代表首字母缩写）、applet、basefont（默认字体）、big（大字体）、center（水平居中）、dir、font（字体标签）、strike（中横线）、tt（文本等宽）、u（下划线）
	- 框架集：frame、frameset、noframes

### meta
HTML的\<meta>标签，可提供有关页面的元信息（meta-information），比如针对搜索引擎和更新频度的描述和关键词；meta常用于定义页面的说明，关键字，最后修改日期，和其它的元数据（metadata）。这些元数据将服务于浏览器（如何布局或重载页面），搜索引擎和其它网络服务。
元信息（或元数据）总是以名称/值对的形式被成对传递。
\<meta> 标签位于文档的头部（\<head>\</head>内），不包含任何内容。

|属性|必填或可选|值|描述|
|---|---|---|---|
|content|必填|some_text|定义与 http-equiv 或 name 属性相关的元信息|
|http-equiv|可选|content-type、expires、refresh、set-cookie|把 content 属性关联到 HTTP 头部|
|name|可选|author、description、keywords、generator、revised、others|name属性主要用于描述网页，比如网页的关键词，叙述等。与之对应的属性值为content，content中的内容是对name填入类型的具体描述，便于搜索引擎抓取。|
|scheme|可选|some_text|定义用于翻译 content 属性值的格式|

- name属性提供了名称/值对中的名称。如果没有提供name属性，name名称/值对中的名称会采用http-equiv属性的值。
**"keywords" (关键字)** 是一个经常被用到的名称。它为文档定义了一组关键字，用于告诉搜索引擎，你网页的关键字。某些搜索引擎在遇到这些关键字时，会用这些关键字对文档进行分类。
类似这样的 meta 标签可能对于进入搜索引擎的索引有帮助：
	```html
	<meta name="keywords" content="github，题目收集，前端">
	```
	**"description"(网站内容的描述)** 用于告诉搜索引擎，你网站的主要内容。
	```html
	<meta name="description" content="前端常见面试题目收集整理">
	```
	**"viewport"(移动端的窗口)** 这个属性常用于设计移动端网页。
	```html
	<meta name="viewport" content="width=device-width, initial-scale=1">
	```
- http-equiv属性为名称/值对提供了名称。并指示服务器在发送实际的文档之前先在要传送给浏览器的MIME文档头部包含名称/值对。
当服务器向浏览器发送文档时，会先发送一些名称/值对，其中必然有一条：`content-type:text/html`（这将告诉浏览器准备接受一个HTML文档）。
使用带有http-equiv属性的\<meta>标签时，服务器将把名称/值对添加到发送给浏览器的内容头部。例如，添加：
	```html
	<meta http-equiv="charset" content="iso-8859-1">
	<meta http-equiv="expires" content="31 Dec 2008">
	```
	这样发送到浏览器的头部就应该包含：
	```
	content-type: text/html
	charset: iso-8859-1
	expires: 31 Dec 2008
	```
	当然，只有当浏览器可以接受这些附加的头部字段，并能以适当的方式使用它们时，这些字段才有意义。
	**在HTML5中，有一个新的 charset 属性，它使字符集的定义更加容易：**
	```html
	<!-- html 4.01 -->
	<meta http-equiv="content-type" content="text/html; charset=ISO-8859-1">
	<!-- html5 -->
	<meta charset="ISO-8859-1">
	```
- content属性提供了名称/值对中的值。改值可以是任何有效的字符串。
content属性始终要和name属性或http-equiv属性一起使用。
- scheme属性用于指定翻译属性值的方案。该方案应该在由\<head>标签的profile属性指定的概况文件中进行了定义。**在HTML5中，不再支持scheme属性。**


常用的名称/值对：

```html
<!-- 设置缩放 -->
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no, minimal-ui" />
<!-- 可隐藏地址栏，仅针对IOS的Safari（注：IOS7.0版本以后，safari上已看不到效果） -->
<meta name="apple-mobile-web-app-capable" content="yes" />
<!-- 仅针对IOS的Safari顶端状态条的样式（可选default/black/black-translucent ） -->
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<!-- IOS中禁用将数字识别为电话号码/忽略Android平台中对邮箱地址的识别 -->
<meta name="format-detection" content="telephone=no, email=no" />

<!-- 其他meta标签 -->
<!-- 启用360浏览器的极速模式(webkit) -->
<meta name="renderer" content="webkit">
<!-- 避免IE使用兼容模式 -->
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<!-- 针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓 -->
<meta name="HandheldFriendly" content="true">
<!-- 微软的老式浏览器 -->
<meta name="MobileOptimized" content="320">
<!-- uc强制竖屏 -->
<meta name="screen-orientation" content="portrait">
<!-- QQ强制竖屏 -->
<meta name="x5-orientation" content="portrait">
<!-- UC强制全屏 -->
<meta name="full-screen" content="yes">
<!-- QQ强制全屏 -->
<meta name="x5-fullscreen" content="true">
<!-- UC应用模式 -->
<meta name="browsermode" content="application">
<!-- QQ应用模式 -->
<meta name="x5-page-mode" content="app">
<!-- windows phone 点击无高光 -->
<meta name="msapplication-tap-highlight" content="no">
```

更多：https://segmentfault.com/a/1190000004279791

### viewpoint


```html
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no" />
<!-- 
width    	设置viewport宽度，为一个正整数，或字符串‘device-width’
device-width  	设备宽度
height   	设置viewport高度，一般设置了宽度，会自动解析出高度，可以不用设置
initial-scale   默认缩放比例（初始缩放比例），为一个数字，可以带小数
minimum-scale   允许用户最小缩放比例，为一个数字，可以带小数
maximum-scale   允许用户最大缩放比例，为一个数字，可以带小数
user-scalable   是否允许手动缩放 
-->
```

### 如何处理移动端1px被处理成2px的问题
	1. 局部处理：meta标签中的viewport属性，initial-scale设置为1。rem按照设计稿标准走，外加利用transform的scale(0.5)缩小一倍即可；
	2. 全局处理：meta标签的viewport属性，initial-scale设置为0.5，rem按照设计稿标准走即可。

### audio元素和video元素在iOS和Android中无法自动播放
### HTML和XHTML区别
### 移动端布局方式，rem布局原理（width=device-width）

未完待续……

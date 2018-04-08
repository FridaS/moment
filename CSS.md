[TOC]

### CSS常用布局
### BFC
### 水平居中、垂直居中
### px/em/rem区别
### animate、transition区别，animate如何停留在最后一帧
### 响应式布局（包括但不限于media query）
### 用css画三角形（直角三角形、等腰三角形等）
### css动画和js动画的区别？动画为什么尽量用CSS而不是js实现？（性能？）
### css动画相关  transform、transition、animation
### 浏览器如何优化动画
### 字体font-family
```css
@ 宋体      SimSun
@ 黑体      SimHei
@ 微信雅黑   Microsoft Yahei
@ 微软正黑体 Microsoft JhengHei
@ 新宋体    NSimSun
@ 新细明体  MingLiU
@ 细明体    MingLiU
@ 标楷体    DFKai-SB
@ 仿宋     FangSong
@ 楷体     KaiTi
@ 仿宋_GB2312  FangSong_GB2312
@ 楷体_GB2312  KaiTi_GB2312  
@
@ 说明：中文字体多数使用宋体、雅黑，英文用Helvetica
    
body { font-family: Microsoft Yahei,SimSun,Helvetica; }
```

font-family规定元素的字体系列。font-family可以把多个字体名称作为一个“回退”系统来保存，如果浏览器不支持第一个子图，则会尝试下一个。也就是说，font-family属性的值是用于某个元素的字体族名称或/及类族名称的一个优先表。浏览器会使用它可识别的第一个值。
有两种类型的字体系列名称：
- 指定的系列名称：具体子图的名称，如“times”、“courier”、“arial”；
- 通常字体系列名称：如“serif”、“sans-serif”、“cursive”、“fantasy”、“monospace”。

提示：使用逗号分隔每个值，并始终提供一个类族名称作为最后的选择。
注意：使用某种特定的字体系列（Geneva）完全取决于用户机器上该字体系列是否可用；这个属性没有指示任何字体下载。因此，强烈推荐使用一个通用字体系列名作为后路。

### 消除transition闪屏
```css
.css {
    -webkit-transform-style: preserve-3d;
    -webkit-backface-visibility: hidden;
    -webkit-perspective: 1000;
}
```
过渡动画（在没有启动硬件加速的情况下）会出现抖动的现象， 以上的解决方案只是改变视角来启动硬件加速的一种方式；
启动硬件加速的 另外一种方式：
```css
.css {
    -webkit-transform: translate3d(0,0,0);
    -moz-transform: translate3d(0,0,0);
    -ms-transform: translate3d(0,0,0);
    transform: translate3d(0,0,0);
}
```
启动硬件加速最常用的方式：translate3d、translateZ、transform

opacity属性/过渡动画（需要动画执行的过程中才会创建合成层，动画没有开始或结束后元素还会回到之前的状态）

will-chang属性（这个比较偏僻），一般配合opacity与translate使用（而且经测试，除了上述可以引发硬件加速的属性外，其它属性并不会变成复合层）。

弊端： 硬件加速会导致 CPU性能占用量过大，电池电量消耗加大 ；因此 尽量避免泛滥使用硬件加速。

### 单行文本溢出显示…、多行文本溢出显示…

```css
/* 单行文本溢出显示… */
width:100px;
overflow:hidden;
text-overflow:ellipsis;
white-space:nowrap;

/* 多行文本溢出显示… */
/* 因为使用了webkit的CSS扩展，所以该方法适用于webkit浏览器及移动端 */
display:-webkit-box; /* 将对象作为弹性伸缩盒子模型显示 */
-webkit-box-orient:vertical; /* 设置或检索伸缩盒对象的子元素的排列方式 */
-webkitlline-clamp:2;
overflow:hidden;
```

### 兼容尽量多浏览器的多行文本溢出显示…方式呢？（只能用js来做了？）
### 改变placeholder的字体颜色大小
### flex
### grid：https://www.jianshu.com/p/d183265a8dad
### 盒模型概念
### box-sizing

每个元素都被表示为一个矩形的盒子，标准盒模型（W3C盒模型）和怪异盒模型（IE盒模型）都包括margin、border、padding、content：
- 标准盒模型（W3C盒模型）：cotent仅仅是content、不包含其他部分;盒子实际width = border *2 + padding * 2 + contentWidth
- 怪异盒模型（IE盒模型）：content部分包含了content、border、padding;盒子实际width = contentWidth

考拉页面一般使用标准盒模型
为了解决兼容性，在页面顶部加上DOCTYP声明、以让浏览器支持标准盒模型。
盒模型的转换方式：box-sizing
```
1）content-box：标准盒模型； // 默认值，所以 box-sizing 没有继承性，只有手动写inherit才继承父元素的值，默认是标准和模型
2）border-box：怪异盒模型；
3）inherit：规定从父元素继承 box-sizing 属性的值。
兼容处理：
-webkit-box-sizing:border-box;
-moz-box-sizing:border-box;
box-sizing:border-box;
```

### 行内元素有哪些，块级元素有哪些
### 写出CSS 3的几个属性
### 定位有哪几种
static、relative、absolute、fixed、sticky

### CSS有哪些样式可以给子元素继承
- 可继承：
    - 所有元素可继承：visibility、cursor
    - 内联元素可继承：letter-spacing、word-spacing、white-space、line-height、color、font、font-family、font-size、font-style、font-variant、font-weight、text-decoration、text-transform、direction
    - 块状元素可继承：text-indent、text-align
    - 列表元素可继承：list-style、list-style-type、list-style-position、list-style-image
    - 表格元素可继承：border-collapse
- 不可继承：一般是会改变盒子模型的，如display、margin、border、padding、background、height、min-height、max- height、width、min-width、max-width、overflow、position、left、right、top、 bottom、z-index、float、clear、table-layout、vertical-align、page-break-after、 page-bread-before和unicode-bidi。

参考：(http://blog.163.com/yhwwen@126/blog/static/170468853201326421822/)

### 清除浮动方式、比较
### 说说样式权重的优先级
### background-size contain和cover区别


未完待续……
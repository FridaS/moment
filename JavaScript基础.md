[TOC]


### JavaScript数据类型（其中哪些是基本数据类型）
- number
- string
- boolean
- undefined
- null
- Symbol：ES6新定义，表示独一无二的值
- object

JavaScript中有6种主要类型（语言类型），其中前6种是基本数据类型（简单基本类型），后面是复杂基本类型（引用类型）。

JavaScript内置对象子类型（通常称为内置对象）有：
- Number
- String
- Boolean
- Object
- Function
- Array
- Date
- Regexp
- Error

### null和undefined的差异
### 闭包
### 继承
### js里实现继承，如何避免原型链上面的对象共享
### 节流和防抖
### js的事件执行机制
关于事件循环、执行优先权（macrotask、microtask）

### target和currentTarget
### ajax
（包括XMLHTTPRequest的过程，readyState的集中类型和代表的意思，以及浏览器的兼容处理方案）（即用原生js实现ajax，及其需要注意的要点）：https://juejin.im/post/58c883ecb123db005311861a
### js判断数据类型的方法（及其注意事项和优缺点）
### 函数声明和变量声明（包括ES6）
### this指向问题：
https://github.com/FridaS/blog/issues/9
### new的执行过程
### 面向对象的理解和实现
（ES5和ES6分别的实现方式）
可以从对象的特征：封装、继承、多态说？
### 你认为js这门语言怎么样
弱类型语言通病？对比下其他语言（如Java）？缺点怎么解决？
### js判断设备来源
### 定时器工作机制：
https://johnresig.com/blog/how-javascript-timers-work/
### 事件委托
（Event Delegation，事件代理）：利用事件冒泡和event.target使父节点代理多个子节点的事件（https://www.cnblogs.com/owenChen/archive/2013/02/18/2915521.html）
### requestAnimationFrame及其优点
### DocumentFragment
### 大批量DOM操作对页面渲染的影响以及优化的手段
### 实现图片懒加载：
https://segmentfault.com/a/1190000009755157

### 拖拽功能实现

### setTimeout参数
![](http://chuantu.biz/t6/278/1523274328x-1566657543.png)





待整理：
https://juejin.im/post/58f1fa6a44d904006cf25d22
其中追问1还可以有以下方式：
```
// 也是利用js基本类型的参数传递是按值传递的特性（传给console.log的参数）
for (var i = 0; i < 5; i++) {
let j = i;
    setTimeout(() => {
        console.log(new Date, j);
    }, 1000);
}

console.log(new Date, i);
```



未完待续……
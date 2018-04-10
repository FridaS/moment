[TOC]


### JavaScript数据类型
- number
- string
- boolean
- undefined
- null
- Symbol：ES6新定义，表示独一无二的值
- object

JavaScript中有7种主要类型（语言类型），其中前6种是**基本数据类型（简单基本类型）**，后面是**复杂基本类型（引用类型）**。

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

简单类型由于其占据空间固定，为了提升变量查询速度，将其存储在栈中，即按值访问；引用类型由于其值的大小可能会改变，所以不能将其存放在栈中，否则会降低变量查询速度，因此，将其值存储在堆（heap）中，而存储在变量处的值，是一个指向存储对象的内存的指针、即按地址访问。

### js判断数据类型的方法（及其注意事项和优缺点）
#### typeof
可以判断number、string、boolean、undefined、symbol、object、function。

![](http://chuantu.biz/t6/278/1523274938x-1566657543.png)

总结下：
* 对于简单基本类型，除了null，都能返回正确的结果；
* 对于引用类型，除了function，都返回“object”；
* 对于null，返回“object”；
* 对于function，返回“function”

其中，null有属于自己的数据类型；引用类型中的数组、日期、正则也都有属于自己的具体类型，而typeof对于这些类型的处理，只返回了处于其原型链最顶端的Object类型。

#### instanceof
用来判断A是否为B的实例，它检测的是原型。我们用一段伪代码来模拟其内部执行过程：
```javascript
instanceof(A,B) = {
    var L = A.__proto__;
    var R = B.prototype;
    if(L === R){
        Return true;
    }
    return false;
}
```
instanceof只能用来判断两个对象是否属于实例关系，而不能判断一个对象实例具体属于哪种类型。
![](http://chuantu.biz/t6/278/1523275082x-1566657543.png)

对于数组，instanceof 操作符存在一个问题：它假定只有一个全局执行环境，如果网页中包含多个框架，那实际上就存在两个以上不同的全局执行环境，从而存在两个以上不同版本的构造函数。如果你从一个框架向另一个框架传入一个数组，那么传入的数组与在第二个框架中原生创建的数组分别属于各自不同的构造函数。

```javascript
var iframe = document.createElement('iframe');
document.body.appendChild(iframe);
xArray = window.frames[0].Array;
var arr =new xArray(1,2,3);// [1,2,3]
arr instanceof Array;// false
```
针对数组的这个问题，ES5.1提供了Array.isArray()方法
![](http://chuantu.biz/t6/278/1523275144x-1566657543.png)
其Polyfill：
```javascript
if(!Array.isArray){
    Array.isArray = function(arg) {
        return Object.prototype.toString.call(arg) === ‘[object Array]’;
    };
}
```
Array.isArray()本质上检测的是对象的\[[Class]]值，\[[Class]]是对象的一个内部属性，里面包含了对象的类型信息，其格式为[object ***]，***就是对应的具体类型。对于数组而言，\[[Class]]的值就是[Object Array]。

#### constructor
当一个函数F被定义时，js引擎会为F添加prototype原型，然后在prototype上添加一个constructor属性，并让其指向F的引用。如：
![](http://chuantu.biz/t6/278/1523276038x-1566657543.png)
当执行到var f = new F()时，F被当成了构造函数，f是F的实例对象，此时F原型上的constructor传递到了f上，因此f.constructor == F。

同样，js中的内置对象在内部构建时也是这样做的：
![](http://chuantu.biz/t6/278/1523276085x-1566657543.png)

细节问题：
* null和undefined是无效的对象（没有对应的内置对象），因此它们没有constructor存在，这两种类型的数据需要通过其他方式来判断；
* 函数的constructor是不稳定的，这个主要体现在自定义对象上，当开发者重写prototype后，原有的constructor引用会丢失，constructor会默认为Object：

![](http://chuantu.biz/t6/278/1523276121x-1566657543.png)

这是因为prototype被重新赋值的是一个{}，{}是new Object()的字面量，因为new Object()会将Object原型上的constructor传递给{}，也就是Object本身。
因此，为了规范开发，在重写对象原型时一般都需要重新给constructor赋值，以保证对象实例的类型不被篡改。

#### toString
toString()是Object的原型方法，调用该方法，默认返回当前对象的\[[Class]]。
对于Object对象，直接调用toString()就能返回[object Object]。而对于其他对象，则需要call/apply来调用才能返回正确地类型信息。
![](http://chuantu.biz/t6/278/1523276180x-1566657543.png)

参考：https://www.cnblogs.com/onepixel/p/5126046.html

### null和undefined的差异
### 浅拷贝和深拷贝的区别，深拷贝如何实现
浅拷贝、深拷贝的概念只针对Object、Array等复杂对象。浅拷贝只复制一层对象的属性，而深拷贝则复制了所有层级。
```javascript
// 简单的浅拷贝实现
function shallowCopy(obj){
    var dst = {};
    for(var prop in obj){
        if(obj.hasOwnProperty(prop)){
            dst[prop] = obj[prop];
        }
    }
    return dst;
}

var obj = {
    a: 1,
    arr: [2,3]
};

var copyObj = shallowCopy(obj)
copyObj; // {a:1, arr:[2,3]}
copyObj.a = 2;
copyObj.arr[1] = 10;
copyObj; // {a:2, arr:[2,10]}
obj; // {a:1, arr:[2,10]}
```
由上面的例子可以知道，浅拷贝只会复制一个对象的自身属性，
![](http://chuantu.biz/t6/278/1523277718x-1404764247.png)
而对于引用类型，变量处存储的是其在内存中的地址，所以浅拷贝会导致 copyObj.arr 和 obj.arr 指向同一块内存，所以导致copyObj对arr进行修改会影响到obj。

这个时候需要对其进行深拷贝，不仅复制原对象的各个属性，而且将原对象各个属性所包含的对象也一次采用深拷贝的方法递归复制到新对象上，这样就不会存在copyObj和obj的arr指向同一个对象的问题。
```javascript
// 简单的深拷贝-当不含有函数时（能被json直接表示的数据结构）
function jsonClone(obj){
    return JSON.parse(JSON.stringify(obj));
}


// 简单的深拷贝
function deepCopy(obj){
    if(Array.isArray(obj)){
        return obj.map(deepCopy)    
    }else if(typeof obj === 'object’){
        var result = {};
        for(var key in obj){
            result[key] = deepCopy(obj[key]);
        }
        return result;
    } else {
        return obj;
    }
}
```
需要注意的是，如果对象层级较深，深拷贝会带来性能上的问题。


### 闭包
### 继承
### js里实现继承，如何避免原型链上面的对象共享
### 节流和防抖及其应用
（scroll、touchmove、mousemove、resize等绑定事件中运用）
### js的事件执行机制
关于事件循环、执行优先权（macrotask、microtask）

### target和currentTarget
### ajax
（包括XMLHTTPRequest的过程，readyState的集中类型和代表的意思，以及浏览器的兼容处理方案）（即用原生js实现ajax，及其需要注意的要点）：https://juejin.im/post/58c883ecb123db005311861a
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
### DOM事件流 捕获、冒泡
### DOM API
### requestAnimationFrame及其优点
### DocumentFragment
### 大批量DOM操作对页面渲染的影响以及优化的手段
### 实现图片懒加载：
https://segmentfault.com/a/1190000009755157

### 拖拽功能实现

### setTimeout参数
![](http://chuantu.biz/t6/278/1523274328x-1566657543.png)

### jQuery链式调用的原理
方法 返回this



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
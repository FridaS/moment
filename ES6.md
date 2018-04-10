<!-- TOC -->

- [箭头函数](#箭头函数)
- [箭头函数this和普通this的区别](#箭头函数this和普通this的区别)
- [箭头函数注意事项](#箭头函数注意事项)
- [let、var、const](#letvarconst)
- [解构赋值](#解构赋值)
- [静态方法、静态属性、私有变量](#静态方法静态属性私有变量)
- [Promise](#promise)
    - [Promise简介](#promise简介)
    - [Promise的实现原理](#promise的实现原理)
    - [Promise的问题](#promise的问题)
- [谈谈你对Promise的理解？和ajax的关系？](#谈谈你对promise的理解和ajax的关系)
- [generator怎么实现](#generator怎么实现)
- [Stream，实现gulp功能](#stream实现gulp功能)
- [async/await（ES7）](#asyncawaites7)

<!-- /TOC -->

### 箭头函数
### 箭头函数this和普通this的区别
普通this是根据函数调用时的上下文决定的；箭头函数的this是根据外层（函数或全局）作用域来决定的。
https://github.com/FridaS/blog/issues/9

### 箭头函数注意事项
* 函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象；【在js中 this 对象的指向是可变的，但是在箭头函数中，它是固定的】
* 不可以当做构造函数，也就是说，不可以使用 new 命令，否则会抛出一个错误；
* 不可以使用arguments对象，该对象在函数体内不存在；如果要用，可以用rest参数代替；
* 不可以使用yield命令，因此箭头函数不能用作Generator函数。

### let、var、const
### 解构赋值
### 静态方法、静态属性、私有变量
静态方法和实例方法的区别：http://blog.csdn.net/tanzhengyu/article/details/51079289
难道是考察class？
### Promise
#### Promise简介
#### Promise的实现原理
#### Promise的问题
如果没有处理Promise的reject，会导致错误被丢进黑洞（http://blog.rangle.io/errors-in-promises/），
好在新版的Chrome和Node 7.x能对未处理的异常给出Unhandled Rejection Warning（https://nodejs.org/api/process.html#process_event_unhandledrejection），
而排查这些错误还需要一些特别的技巧（浏览器：https://www.bennadel.com/blog/3239-logging-and-debugging-unhandled-promise-rejections-in-the-browser.htm，
Node.js：https://www.bennadel.com/blog/3238-logging-and-debugging-unhandled-promise-rejections-in-node-js-v1-4-1-and-later.htm）

### 谈谈你对Promise的理解？和ajax的关系？

### generator怎么实现
http://www.alloyteam.com/2016/02/generators-in-depth/

### Stream，实现gulp功能
### async/await（ES7）
```javascript
// async函数的实现原理，就是将Generator函数和自动执行器包装在一个函数里。
async function fn(args) {
    // ...
}

// 等同于
function fn(args) {
    return spawn(function* () {
        // ...
    });
}

// 下面给出spawn函数的实现
function spawn(genF) {
    return new Promise(function(resolve, reject) {
        const gen = genF();
        function step(nextF) {
            let next;
            try {
                next = nextF();
            } catch (e) {
                return reject(e);
            }
            if(next.done) {
                return resolve(next.value);
            }
            Promise.resolve(next.value).then(function(v){
                step(function() {return gen.next(v);});
            }, function(e){
                step(function() {return gen.throw(e);});
            });
        }
        step(function() {return gen.next(undefined);});
    });
}
```

未完待续……
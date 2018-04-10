[TOC]

### 生命周期：
https://github.com/FridaS/blog/issues/2

### Vue组件data为什么必须是函数
因为同一个组件的各个实例是共享一个data的（该组件的data），如果data是一个对象，则每个组件改变data都会影响其他组件实例，而data是函数，如data(){ return {...} }，每个实例都有其自己的一个对象“{}”，它们不会相互影响。https://cn.vuejs.org/v2/guide/components.html

### 动态设置响应式数据 this.$set()

### 如何设计一些组件，原则是什么，写过什么自豪或眼前一亮的组件
https://imys.net/20170317/write-good-front-end-component.html#%E7%BB%84%E4%BB%B6%E8%AE%BE%E8%AE%A1%E5%8E%9F%E5%88%99、
### 组件间通信：
https://github.com/FridaS/blog/issues/3

### Vue父子组件嵌套时，组件内部的各个生命周期钩子触发先后顺序
http://blog.csdn.net/michael8512/article/details/79031656
![](http://chuantu.biz/t6/278/1523273981x-1404764247.png)

![](http://chuantu.biz/t6/278/1523274017x-1566657543.png)

从上面两张图可以看出，非minxin情况，父组件先执行beforeCreate、created、beforeMount，然后子组件执行beforeCreate、created、beforeMount、mounted，子组件挂载之后父组件才mounted；销毁情况顺序也差不多，先父组件beforeDestroy，然后子组件beforeDestroy、destroyed，子组件都销毁了父组件才destroyed；至于beforeUpdate和updated，如果是独立的状态更新（即该状态只在父组件中或只在子组件中，而不是比如props之类的情况）、则父子组件互不影响、独立执行beforeUpdate和updated，如果是诸如props的情况、则与创建和销毁顺序差不多：父组件先beforeUpdate，然后子组件beforeUpdate、updated，然后父组件updated。

那如果父组件中都有minxin(https://cn.vuejs.org/v2/guide/mixins.html)，minxin总是在组件自身钩子之前调用（官方文档原话：混入对象[即mixin]的钩子将在组件自身钩子之前调用）。

总结：从外到内、再从内到外，mixins先于组件。


### virtual DOM和diff算法的实现
https://github.com/livoras/blog/issues/13、https://www.zhihu.com/question/61064119/answer/183717717、https://segmentfault.com/q/1010000000367833
### vue complier实现
### MVC、MVVM，数据单项绑定和双向绑定
https://segmentfault.com/a/1190000006599500、https://zhoukekestar.github.io/notes/2017/06/07/interview-answers.html
https://zh.wikipedia.org/wiki/MVC、https://www.zhihu.com/question/20148405

### vue数据绑定原理

### 计算属性原理
（http://www.cnblogs.com/kidney/p/7384835.html?utm_source=debugrun&utm_medium=referral、https://segmentfault.com/a/1190000010408657）

### Vue响应式原理（Object.defineProperty）的另一种实现方式：Proxy
（http://es6.ruanyifeng.com/#docs/proxy）
### vue slot使用

### vue-router实现方式（和一般使用方式）
https://zhuanlan.zhihu.com/p/27588422
【注意：hash方式（它与history.pushState类似，会记录浏览器历史，hash是通过window.location.hash来赋值的）要实现类似history.replaceState的功能，vue-router的做法是通过window.location.replace来实现，源码：
```javascript
function replaceHash (path) {
    const i = window.location.href.indexOf(‘#')
    window.location.replace(window.location.href.slice(0, i>0 ? i : 0) + ‘#’ + path)
}
```
】

hash、history区别

### 不使用vue-router时如何利用H5的history API来实现路由跳转
### 单页应用
### ssr
https://ssr.vuejs.org/zh/、https://segmentfault.com/a/1190000007933349、https://juejin.im/entry/58c9f6b6ac502e0058854686
【简单总结：服务端渲染（server side rendering），1.有益于SEO，2.对于客户端网络较慢的情况，服务端渲染有利于提高客户端页面性能优化，3.解决客户端运行在老的或者直接没有JavaScript 的引擎上的情况。技术选型：1.vue-ssr（vue-server-renderer，vue官方的服务端渲染包）；2.Nuxt.js（使用官方的vue-server-renderer包装后的脚手架工具，极大简化了ssr的搭建）】【如果只是为了改善少数页面的seo，可以采用另一种方案：webpack的插件 prerender-spa-plugin + vue插件(包？) vue-meta-info ： https://zhuanlan.zhihu.com/p/29148760 】


### vuex概念
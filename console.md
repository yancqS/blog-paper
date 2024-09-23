---
title: 你可能还不知道console对象
date: '2020-09-04'
cover: 'http://img.up-4ever.site/20200905134629.jpg'
top_img: 'http://img.up-4ever.site/infinity-4164366.jpg'
tags:
  - Javascript
  - 前端
categories:
  - 前端开发
abbrlink: 41713f56
---
# console对象详解

直接进入主题，先看一下console这个对象包含哪些属性。

![](http://img.up-4ever.site/20200905134629.jpg)

其中console对象包含的函数属性：**assert**、**clear**、**context**、**count**、**countReset**、**debug**、**dir**、**dirxml**、**error**、**group**、**groupCollapsed**、**groupEnd**、**info**、**log**、**profile**、**profileEnd**、**table**、**time**、**timeEnd**、**timeLog**、**timeStamp**、**trace**、**warn**。

还有一个属性：**memory**

## 显示信息的命令

最简单常用的就是console.log()。另外，根据显示信息的性质不同，console对象还有4种显示信息的方法，分别是一般信息console.info()、除错信息console.error()、警告信息console.warn()、错误提示console.error()。

![](http://img.up-4ever.site/20200905140013.jpg)

## 占位符

console对象的上面5种方法，都可以使用printf风格的占位符。不过，占位符的种类比较少，只支持字符（%s）、整数（%d或%i）、浮点数（%f）和对象（%o）四种。

比如：

```javascript
console.log("%d年%d月%d日",2011,3,26);
console.log("圆周率是%f",3.1415926);
```

输出为：

![](http://img.up-4ever.site/20200907171132.jpg)

%o占位符，可以用来查看一个对象内部情况。比如，有这样一个对象：

```javascript
let dog = {};
dog.name = '贝贝'；
dog.color = '黄色';

console.log('%o', dog);
```

![](http://img.up-4ever.site/20200907185516.jpg)

还有一种特殊的标示符**%c**，对输出的文字可以附加特殊的样式。

```javascript
console.log('%c酒剑仙', 'color: yellow; background: #ccc; font-size: 20px');
```

![](http://img.up-4ever.site/20200907204757.jpg)


## 分组显示

如果信息太多，可以分组显示，用到的方法是console.group()和console.groupEnd()。还有一个console.groupCollapsed() 是输出组的标题。

```javascript
console.group("First Group");
console.log('1-1');
console.log('1-2');
console.groupEnd();
console.group("Second Group");
console.log('2-1');
console.log('2-2');
console.groupEnd();
```
输出为：

![](http://img.up-4ever.site/20200907185942.jpg)

## 计时功能

console.time([label])和console.timeEnd([label])，用来显示代码的运行时间。console.timeLog()在控制台输出计时器的值，该计时器必须已经通过 console.time([label]) 启动。label默认值为default。

```javascript
console.time('计时器');
let sum = 0;
for(let i = 0; i < 100000; i++){
    sum += i; 
    if(i == 50000) console.timeLog('计时器');
}
console.timeEnd('计时器');
console.log(sum);
```

![](http://img.up-4ever.site/20200908145707.jpg)

## 性能分析

性能分析（Profiler）就是分析程序各个部分的运行时间，找出瓶颈所在，使用的方法是console.profile()。

```javascript
function Foo() {
    for(let i = 1; i < 20; i++) {
        fn(i);
    }
}
function fn(n) {
    sum(n);
}
function sum(m) {
    for(let i = 0; i < m; i++) {
        console.log(Math.pow(2, i + 1));
    }
}
console.profile('Profile Foo')
Foo();
console.profileEnd('Profile Foo');
```

在Javascript Profile就可以看到：

![](http://img.up-4ever.site/20200907212855.jpg)

## 断言

console.assert(expression, object[, object...])当expression为false时输出后续参数，当expression为true时无输出。

![](http://img.up-4ever.site/20200907213249.jpg)

## 计数

console.count([label])输出执行到改行的**次数**，可选参数label输出在次数之前。默认label为default。

console.countReset([label]);重置次数。

![](http://img.up-4ever.site/20200908125709.jpg)

重置次数：

![](http://img.up-4ever.site/20200908131023.jpg)


## 表格
console.table();可以将传入的对象或者数组以表格的形式输出。相比树形输出，这种输出方案更适合内部元素排列整齐的对象或数组，不然会出现很多undefined。

```javascript
let obj = {
    foo:{
        name:'yoha',
        age:12
    },
    baz:{
        name:'yooo',
        age:13 
    }
}
console.table(obj);
```

输出为：

![](http://img.up-4ever.site/20200908130343.jpg)

## console.context()

打印出所有的console对象的**方法**(除了本身console.context()不会被打印出来)

![](http://img.up-4ever.site/20200908170336.jpg)

## console.trace()

```javascript
function add(a,b){
    console.trace();
    return a+b;
}
let x = add3(1,1);
function add3(a,b){return add2(a,b);}
function add2(a,b){return add1(a,b);}
function add1(a,b){return add(a,b);}
```

![](http://img.up-4ever.site/20200908172008.jpg)

## 其他

- console.clear() 用于清空控制台(当然你可以用快捷键ctrl+L)。
- console.timeStamp()  向浏览器的Performance或者Waterfall工具添加一个标记。这样可以让你将代码中的一个点和其他在时间轴上已记录的事件相关联，例如布局事件和绘制事件等。
- console.dir() 可以打印对象,包括普通对象，函数，数组等，还可以打印DOM元素。
- consoe.dirxml() 打印XML/HTML,包括DOM元素或者Object对象。

![](http://img.up-4ever.site/20200908171341.jpg)

### 一个属性

console.memory

![](http://img.up-4ever.site/20200908170627.jpg)

## 分享题目

```javascript
function f1() {
    console.time('time span');
}
function f2() {
    console.timeEnd('time span');
}
setTimeout(f1, 100);
setTimeout(f2, 200);

function waitForMs(n) {
    let now = Date.now();
    while (Date.now() - now < n) {}
}
waitForMs(500);//time span: 0.022216796875 ms
```

其实这个题目考察的主要是JS的事件循环模型。setTimeout(fn,timer)是异步代码，执行到setTimeout的时候会有定时器线程进行计时，计时完毕将回调函数推到事件队列，等待主线程执行完毕，然后将事件队列的第一个压入到执行栈。因此setTimeout()执行的条件有两个：1.定时器线程计时完毕，2.主线程空闲。

因此上面代码在500ms后，f1()和f2()都已经计时完毕，已经推入事件队列。所以主线程一空闲，这两个函数几乎是同时执行的。这也就是输出不是100ms的原因了。



## 参考文章

- [console命令详解](https://www.cnblogs.com/pengfei25/p/6019525.html)
- [使用console进行 性能测试 和 计算代码运行时间](https://www.cnblogs.com/0603ljx/p/4387628.html)
- [你真的了解 console 吗](https://segmentfault.com/a/1190000000481884)
- [扒一扒那些年我们console过的那些事儿](https://www.cnblogs.com/kalkin/p/11082808.html)
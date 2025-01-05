---
title: 事件触发器EventEmitter
date: '2020-06-23'
tags:
  - 发布订阅模式
categories:
  - 设计模式
cover: 'http://img.up-4ever.site/20201017114430.jpg'
top_img: 'http://img.up-4ever.site/infinity-1115505.jpg'
abbrlink: c9795266
---
# 事件触发器EventEmitter

## 发布订阅模式：

```javascript
class EventEmitter {
  constructor() {
    this._event = []
  }

  on(eventName, fn) {
    let callback = this._event[eventName] || [];
    callback.push(fn);
    this._event[eventName] = callback;
    return this;
  }

  emit(...args) {
    let eventName = args[0];
    let params = args.slice(1);
    let event = this._event[eventName];
    event.forEach(fn => fn.apply(this, params));
    return this;
  }

  off(eventName, fn) {
    let callback = this._event[eventName];
    this._event[eventName] = callback && callback.filter(_fn => _fn !== fn);
    return this;
  }

  once(eventName, fn) {
    let wrapFun = function(...args) {
      fn.apply(this, args);
      this.off(eventName, wrapFun);
    }
    this.on(eventName, wrapFun);
    return this;
  }
}

let event = new EventEmitter();
function handleClick(num) {
  console.log('--click--', num);
}
function handleClick1(num) {
  console.log('--click1--', num);
}
event.on('click', handleClick);//订阅（监听）
event.on('click', handleClick1);//订阅（监听）
/*执行其他代码，在某个时间触发（发布）click信号*/
...
event.emit('click', 10);//发布（触发）
event.emit('click', 100);//发布（触发）
```

上面的eventEmitter类的实现就是基于发布订阅模式。在阮一峰老师的《Javascript异步编程的4种方法》中提到：

>我们假定，存在一个"信号中心"，某个任务执行完成，就向信号中心"发布"（publish）一个信号，其他任务可以向信号中心"订阅"（subscribe）这个信号，从而知道什么时候自己可以开始执行。这就叫做"发布/订阅模式"（publish-subscribe pattern），又称"观察者模式"（observer pattern）。

发布订阅模式可以理解为是观察者模式的一种变形，两者的区别是：发布/订阅模式在观察者模式的基础上，在目标和观察者之间增加一个调度中心。

![](http://img.up-4ever.site/20201017114430.jpg)

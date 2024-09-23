---
title: JS原生函数实现之数组实例方法map/reduce
date: '2020-06-16'
tags:
  - JavaScript
categories:
  - JavaScript
top_img: 'http://img.up-4ever.site/20210420224048.jpeg'
cover: 'http://img.up-4ever.site/infinity-12769278.jpg'
abbrlink: 7841ce51
---
# JS原生函数实现之数组实例方法map/reduce

![](http://img.up-4ever.site/20201017150312.png)

```javascript
Array.prototype.map2 = function (fn, thisVal) {
  if (typeof fn != 'function') {
    throw new Error(fn + 'should be function');
  }
  if (this.length == 0) {
    return [];
  }
  return this.reduce((arr, curVal, curIndex) => {
    arr.push(fn.call(thisVal, curVal, curIndex, this));
    return arr;
  }, [])
};
```

![](http://img.up-4ever.site/20201017150326.png)

```javascript
Array.prototype.reduce2 = function (callback, initData) {
  let initVal = initData ? initData : initData == 0 ? initData : this[0];
  for (let i = initData ? 0 : initData == 0 ? 0 : 1; i < this.length; i++) {
    initVal = callback(initVal, this[i], i, this)
  }
  return initVal;
}
```

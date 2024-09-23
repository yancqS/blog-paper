---
title: 使用JS生成UUID的常用方法
date: '2022-10-22'
cover: 'http://img.up-4ever.site/20221023004047.png'
top_img: 'http://img.up-4ever.site/infinity-9909115.jpg'
tags:
  - JavaScript
  - 转载
categories:
  - JavaScript
abbrlink: 68ab5d33
---

# 使用JS生成UUID的常用方法

> UUID 是 通用唯一识别码（Universally Unique Identifier）的缩写，是一种软件建构的标准，亦为开放软件基金会组织在分布式计算环境领域的一部分。

## 一、使用 Math.random()

```js
function getUuid() {
  return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
    var r = (Math.random() * 16) | 0,
      v = c == 'x' ? r : (r & 0x3) | 0x8;
    return v.toString(16);
  });
}
```

## 二、使用crypto提供的randomUUID方法

> [MDN Cropto API](https://developer.mozilla.org/zh-CN/docs/Web/API/Crypto)

```js
const UUID = crypto.randomUUID();
```

## 三、使用crypto提供的getRandomValues方法

```js
function getUuid() {
    return ([1e7]+-1e3+-4e3+-8e3+-1e11).replace(/[018]/g, c => (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16));
}
```

## 实际开发中推荐的方法

```ts
export const uuidv4 = () => {
  if (typeof crypto === 'object') {
    if (typeof crypto.randomUUID === 'function') {
      return crypto.randomUUID();
    }
    if (typeof crypto.getRandomValues === 'function' && typeof Uint8Array === 'function') {
      const callback = (c: string) => {
        const num = Number(c);
        return (num ^ (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (num / 4)))).toString(16);
      };
      return (`${1e7}-${1e3}-${4e3}-${8e3}-${1e11}`).replace(/[018]/g, callback);
    }
  }
  let timestamp = new Date().getTime();
  let perforNow = (typeof performance !== 'undefined' && performance.now && performance.now() * 1000) || 0;
  return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, (c) => {
    let random = Math.random() * 16;
    if (timestamp > 0) {
      random = (timestamp + random) % 16 | 0;
      timestamp = Math.floor(timestamp / 16);
    } else {
      random = (perforNow + random) % 16 | 0;
      perforNow = Math.floor(perforNow / 16);
    }
    return (c === 'x' ? random : (random & 0x3) | 0x8).toString(16);
  });
};
```

## 最后

本文转载于[使用JS生成UUID的常用方法](https://zhuanlan.zhihu.com/p/469763011), 对作者表示感谢~

---
title: 如何实现并发请求数量控制？
description: 如何实现并发请求数量控制？
date: '2022-11-20'
cover: 'http://img.up-4ever.site/20221120191733.webp'
top_img: 'http://img.up-4ever.site/infinity-4164366.jpg'
tags:
  - 前端
  - JavaScript
  - 转载
categories:
  - 前端开发
abbrlink: d677079f
---

# 如何实现并发请求数量控制？

![](http://img.up-4ever.site/20221120191733.webp)

## 前言

本文为转载文章，原文地址：[关于前端：如何实现并发请求数量控制?](https://juejin.cn/post/7163522138698153997)

对文章作者表示感谢。

> 文章代码实现部分根据理解做了部分修改，整体效果无差异。

## 场景

假设有这么一个场景：现在有`20`个异步请求需要发送，但是由于某些原因，要求我们必须将同一时刻的并发请求数量控制在`3`
个以内，并且还要尽可能快速的拿到响应结果。其实这个场景在一些大厂的面试题中也有过提及，如下：

```
实现一个并发请求函数 concurrencyRequest(urls, maxNum)，要求如下：
• 要求最大并发数 maxNum
• 每当有一个请求返回，就留下一个空位，可以增加新的请求
• 所有请求完成后，结果按照 urls 里面的顺序依次打出（发送请求的函数可以直接使用fetch即可）
```

## 设计思路

首先来看将上面的文字转化为图之后的效果：

![](http://img.up-4ever.site/20221120192634.webp)

这样就直观的看到，有一个最大并发数`maxNum`，`20`个异步请求的`urls`集合和并发返回之后的`results`集合。

下面就开始演示这个思路是如何开始的，如下：

![](http://img.up-4ever.site/20221120192847.webp)

首先按照每次只能并发`3`个请求的要求，这里就对应`A、B、C`，当其中有一个请求完之后就会再从`urls`里面再取出一个进行请求，这样依次类推，直到
`urls`里面的`20`个请求都执行完才终止请求。

![](http://img.up-4ever.site/20221120193051.webp)

主要思路就是上面所述，但是在开发时我们要考虑一些特殊情况，如下：

- `urls`的长度为`0`时，`results`就没有值，此时应该返回空数组
- `maxNum`大于`urls`的长度时，应该取的是`urls`的长度，否则则是取`maxNum`
- 因为没有考虑请求是否请求成功，所以请求成功或报错都应把结果保存在`results`集合中
- `results`中的顺序需和`urls`中的保持一致

## 开发

具体代码如下：

> 非原文代码，2024-09-21 修改

```js
const currencyRequest = (urls: string[], limit: number) => {
  return new Promise(async (resolve) => {
    if (!urls.length) {
      resolve([]);
      return;
    }
    let index = -1;
    const result: unknown[] = [];
    const request = async () => {
      if(index === urls.length - 1) return;
      index++;
      const i = index;
      console.log(index);
      try {
        const res = await fetch(urls[index]);
        result[i] = await res.json();
      } catch (e) {
        result[i] = e;
      } finally {
        if (i === urls.length - 1) {
          resolve(result);
        } else {
          request();
        }
      }
    }
    const times = Math.min(limit, urls.length);
    for (let i = 0; i < times; i++) {
      request();
    }
  })
}
```

## 测试

在上面代码下添加如下代码：

```js
(async () => {
  const urls = [];
  const maxNum = 3;
  for (let i = 1; i <= 20; i++) {
    urls.push(`https://jsonplaceholder.typicode.com/todos/${i}`);
  }
  console.time('request');
  const res = await concurrencyRequest(urls, maxNum);
  console.timeEnd('request');
  console.log(res);
})()
```

## 结果

![](http://img.up-4ever.site/20221120195105.png)
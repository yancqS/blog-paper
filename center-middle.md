---
title: 元素垂直水平居中的几种方法
date: '2020-06-17'
cover: 'http://img.up-4ever.site/20201017145747.jpeg'
top_img: 'http://img.up-4ever.site/infinity-1901105%20(1).jpg'
tags:
  - 前端开发
  - CSS
categories:
  - CSS
abbrlink: 77eb9d4a
---
# 元素垂直水平居中的几种方法

HTML:

```html
<body>
  <div></div>
</body>
```

## 方法1：

```css
body {
  width: 100%;
  height: 100vh;
  border: 1px solid blue;
}
body div {
  height: 100px;
  width: 100px;
  background-color: red;
  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  margin: auto;
}
```

## 方法2：

```css
body {
  width: 100%;
  height: 100vh;
  border: 1px solid blue;
  display: flex;
}
body div {
  height: 100px;
  width: 100px;
  background-color: red;
  margin: auto;
}
```

## 方法3：

```css
body {
  width: 100%;
  height: 100vh;
  border: 1px solid blue;
  display: flex;
  justify-content: center;
  align-items: center;
}
body div {
  height: 100px;
  width: 100px;
  background-color: red;
}
```

## 方法4：

```css
body {
  width: 100%;
  height: 100vh;
  border: 1px solid blue;
}
body div {
  height: 100px;
  width: 100px;
  background-color: red;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
```

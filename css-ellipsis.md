---
title: 单行省略和多行省略
description: 单行省略和多行省略
date: '2020-12-20'
cover: 'http://img.up-4ever.site/20201219170044.gif'
top_img: 'http://img.up-4ever.site/infinity-6462079.jpg'
tags:
  - 转载
  - CSS
categories:
  - CSS
abbrlink: c46fe94b
---
# 单行省略和多行省略

>转载自[可能是最全的 “文本溢出截断省略” 方案合集](https://www.zoo.team/article/text-overflow)。对其内容进行了学习验证和修改(JS实现多行省略部分)。对文章作者[政采云前端团队](https://www.zoo.team/)表示感谢。

## 前言

在我们的日常开发工作中，文本溢出截断省略是很常见的一种需考虑的业务场景细节。看上去 “稀松平常” ，但在实现上却有不同的区分，是单行截断还是多行截断？多行的截断判断是基于行数还是基于高度？这些问题之下，都有哪些实现方案？他们之间的差异性和场景适应性又是如何？凡事就怕较真，较真必有成长。本文试图通过编码实践，给出一些答案。

## 单行省略

**核心CSS语句**

-  overflow: hidden; (文字超出限定宽度，则隐藏超出的内容)
-  text-overflow: ellipsis; (规定当文本溢出时，显示省略符号来代表被修剪的文本)
-  white-space: nowrap; (设置文字在一行显示，不能换行)

**优点**

- 无兼容问题
- 响应式截断
- 文本溢出范围才显示省略号，否则不显示省略号
- 省略号显示的位置刚好

**短板**

- 只支持单行文本截断

**使用场景**

- 适用于单行文本溢出显示省略号的情况

DEMO:

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .singleline {
            width: 100%;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }
    </style>
</head>
<body>
    <p>
        单行省略：
    </p>
    <div class="singleline">
        中国青年报客户端北京12月18日电（中青报·中青网记者叶雨婷）今天记者从教育部获悉，教育部印发《关于正确认识和规范使用高校人才称号的若干意见》的通知。通知指出，坚持人岗相适原则延揽人才，不将人才称号作为硬性指标，不针对人才称号获得者发布“明码标价”的招聘广告。要统筹用好国内外人才资源，不将国（境）外学习或工作经历作为人才招聘引进的限制性条件。
    </div>
</body>
```

实例图片：

![](http://img.up-4ever.site/20201219141849.gif)

## 多行文本溢出省略(按行数)

### 纯CSS实现方案

**核心CSS语句**

- -webkit-line-clamp: 3; (可以把 块容器 中的内容限制为指定的行数)
>它只有在 display 属性设置成 -webkit-box 或者 -webkit-inline-box 并且 -webkit-box-orient 属性设置成 vertical时才有效果。
- display: -webkit-box;
- -webkit-box-orient: vertical;
- overflow: hidden;
>在大部分情况下,也需要设置 overflow 属性为 hidden, 否则,里面的内容不会被裁减,并且在内容显示为指定行数后还会显示省略号(ellipsis ).

**优点**

- 响应式截断
- 文本溢出范围才显示省略号，否则不显示省略号
- 省略号显示位置刚好

**短板**

- 兼容性一般： -webkit-line-clamp 属性只有 WebKit 内核的浏览器才支持

![](http://img.up-4ever.site/20201219144854.jpg)

>截图自[caniuse](https://caniuse.com/)

**适用场景**

- 多适用于移动端页面，因为移动设备浏览器更多是基于 WebKit 内核

**DEMO：**

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .multiline {
            width: 100%;
            display: -webkit-box;
            -webkit-box-orient: vertical;
            -webkit-line-clamp: 3;
            overflow: hidden;
        }
    </style>
</head>

<body>
    <p>
        多行省略：
    </p>
    <div class="multiline">
        中国青年报客户端北京12月18日电（中青报·中青网记者叶雨婷）今天记者从教育部获悉，教育部印发《关于正确认识和规范使用高校人才称号的若干意见》的通知。通知指出，坚持人岗相适原则延揽人才，不将人才称号作为硬性指标，不针对人才称号获得者发布“明码标价”的招聘广告。要统筹用好国内外人才资源，不将国（境）外学习或工作经历作为人才招聘引进的限制性条件。
    </div>
</body>
```

![](http://img.up-4ever.site/20201219142333.gif)

### 基于Javascript的实现方案

**优点**

- 无兼容问题
- 响应式截断
- 文本溢出范围才显示省略号，否则不显示省略号

**短板**

- 需要JS实现，背离展示和行为相分离的原则
- 文本为中英文/纯英文时，省略号的位置会有偏差

**使用场景**

- 适用于响应式截断，多行文本溢出省略的情况。（最好为纯中文）

**DEMO**

```html
<body>
    <p>
        多行省略：
    </p>
    <div class="multiline"></div>
</body>
<script>
    const text = '中国青年报客户端北京12月18日电（中青报·中青网记者叶雨婷）今天记者从教育部获悉，教育部印发《关于正确认识和规范使用高校人才称号的若干意见》的通知。通知指出，坚持人岗相适原则延揽人才，不将人才称号作为硬性指标，不针对人才称号获得者发布“明码标价”的招聘广告。要统筹用好国内外人才资源，不将国（境）外学习或工作经历作为人才招聘引进的限制性条件。';
    const textTotalLength = text.length;
    const formatStr = () => {
        const ele = document.getElementsByClassName('multiline')[0];
        //要显示的行数
        const lineNum = 3;
        const baseWidth = parseFloat(window.getComputedStyle(ele, null).width);
        const baseFontSize = parseFloat(window.getComputedStyle(ele, null).fontSize);
        //一行显示多少个字
        const strNum = baseWidth / baseFontSize;
        let content = '';
        //多行可容纳的总字数
        const totalStrNum = lineNum * strNum;
        if (textTotalLength > totalStrNum) {
            content = text.slice(0, totalStrNum - 1) + '...';
        } else {
            content = text;
        }
        ele.innerHTML = content;
    }
    formatStr();
    window.onresize = formatStr;
</script>
```

![](http://img.up-4ever.site/20201219151507.gif)

## 多行文本溢出省略(按高度)

### 多行文本溢出不显示省略号

**核心CSS语句**

- overflow: hidden;
- max-height: 60px; (设定当前元素最大高度)
- line-height: 20px; (结合元素高度，高度固定的情况下，设定行高， 控制显示行数)

**优点**

- 无兼容问题
- 响应式截断

**短板**

- 单纯的截断文字，不展示省略号，观感上较为生硬

**使用场景**

- 适用于文本溢出不需要显示省略号的情况

**DEMO**

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .multiline {
            overflow: hidden;
            max-height: 60px;
            line-height: 20px;
        }
    </style>
</head>
<body>
    <p>
        多行省略：
    </p>
    <div class="multiline">
        中国青年报客户端北京12月18日电（中青报·中青网记者叶雨婷）今天记者从教育部获悉，教育部印发《关于正确认识和规范使用高校人才称号的若干意见》的通知。通知指出，坚持人岗相适原则延揽人才，不将人才称号作为硬性指标，不针对人才称号获得者发布“明码标价”的招聘广告。要统筹用好国内外人才资源，不将国（境）外学习或工作经历作为人才招聘引进的限制性条件。
    </div>
</body>
```

![](http://img.up-4ever.site/20201219160254.gif)

### 伪元素 + 定位实现多行省略

**核心CSS语句**
- position: relative; （为伪元素绝对定位）
- overflow: hidden; （文本溢出限定的宽度就隐藏内容）
- position: absolute;（给省略号绝对定位）
- line-height: 20px; （结合元素高度,高度固定的情况下,设定行高, 控制显示行数）
- height: 40px; （设定当前元素高度）
- ::after {} （设置省略号样式）

**优点**

- 无兼容问题
- 响应式截断

**短板**

- 无法识别文字的长短，无论文本是否溢出范围，都会一直显示省略号
- 省略号的位置可能不是刚刚好，又是可能会遮住一半的文字。(可以用线性渐变背景色来解决)

**适用场景**

- 适用于对省略效果要求较低，文本一定会溢出元素的情况

**DEMO**

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .multiline {
            position: relative;
            overflow: hidden;
            max-height: 60px;
            line-height: 20px;
        }
        .multiline::after {
            content: '...';
            position: absolute;
            bottom: 0;
            right: 0;
            padding-left: 30px;
            background-image: linear-gradient(to right, rgba(255,255,255,0.8), rgba(255,255,255,1));
        }
    </style>
</head>
<body>
    <p>
        多行省略：
    </p>
    <div class="multiline">
        中国青年报客户端北京12月18日电（中青报·中青网记者叶雨婷）今天记者从教育部获悉，教育部印发《关于正确认识和规范使用高校人才称号的若干意见》的通知。通知指出，坚持人岗相适原则延揽人才，不将人才称号作为硬性指标，不针对人才称号获得者发布“明码标价”的招聘广告。要统筹用好国内外人才资源，不将国（境）外学习或工作经历作为人才招聘引进的限制性条件。
    </div>
</body>
```


![](http://img.up-4ever.site/20201219170044.gif)

### 利用Float特性，纯CSS实现多行省略

**核心CSS**

- line-height: 20px;（结合元素高度,高度固定的情况下,设定行高, 控制显示行数）
- overflow: hidden;（文本溢出限定的宽度就隐藏内容）
- float: right/left;（利用元素浮动的特性实现）
- position: relative;（根据自身位置移动省略号位置, 实现文本溢出显示省略号效果）
- word-break: break-all;（使一个单词能够在换行时进行拆分）

**优点**

- 无兼容问题
- 响应式截断
- 文本溢出范围显示省略号，否则不显示

**短板**

- 省略号显示可能不会刚刚好，有时会遮住一半文字

**适用场景**

- 适用于对省略效果要求较低，多行文本响应式截断的情况

**DEMO**

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .multiline {
            overflow: hidden;
            max-height: 60px;
            line-height: 20px;
        }
        .multiline::before {
            float: left;
            content: '';
            width: 20px;
            height: 60px;
        }
        .multiline .text {
            float: right;
            width: 100%;
            margin-left: -20px;
            word-break: break-all;
        }
        .multiline::after {
            float: right;
            content: '...';
            width: 20px;
            height: 20px;
            position: relative;
            left: 100%;
            transform: translate(-100%, -100%);
            background-image: linear-gradient(to right, rgba(255,255,255,0.8), rgba(255,255,255,1));
        }
    </style>
</head>

<body>
    <p>
        多行省略：
    </p>
    <div class="multiline">
        <div class="text">
            中国青年报客户端北京12月18日电（中青报·中青网记者叶雨婷）今天记者从教育部获悉，教育部印发《关于正确认识和规范使用高校人才称号的若干意见》的通知。通知指出，坚持人岗相适原则延揽人才，不将人才称号作为硬性指标，不针对人才称号获得者发布“明码标价”的招聘广告。要统筹用好国内外人才资源，不将国（境）外学习或工作经历作为人才招聘引进的限制性条件。
        </div>
    </div>
</body>
```

![](http://img.up-4ever.site/20201219190554.gif)

**原理讲解**

有 A、text、C 三个盒子，A 左浮动，text、C 右浮动。设置 A 盒子的高度与 B 盒子高度（或最大高度）要保持一致, A盒子的宽度大于(或等于)C盒子的宽度。

- 当 text 盒子的高度低于 A 盒子，C 盒子仍会处于 B 盒子右下方。
- 如果 text 盒子文本过多，高度超过了 A 盒子，则 C 盒子不会停留在右下方，而是掉到了 A 盒子下。

![](http://img.up-4ever.site/20201219191506.gif)


代码如下：

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .multiline {
            max-height: 60px;
            line-height: 20px;
            position: relative;
        }
        .multiline .AFLeft {
            float: left;
            width: 100px;
            height: 60px;
            background: burlywood;
        }
        .multiline .text {
            float: right;
            width: calc(100% - 100px);
            word-break: break-all;
            background: blue;
        }
        .multiline .CFRight {
            background: yellow;
            float: right;
            width: 100px;
            height: 20px;
        }
    </style>
</head>

<body>
    <p>
        多行省略：
    </p>
    <div class="multiline">
        <div class="AFLeft">A float:left</div>
        <div class="text">
            中国青年报客户端北京12月18日电（中青报·中青网记者叶雨婷）今天记者从教育部获悉，教育部印发《关于正确认识和规范使用高校人才称号的若干意见》的通知。通知指出，坚持人岗相适原则延揽人才，不将人才称号作为硬性指标，不针对人才称号获得者发布“明码标价”的招聘广告。要统筹用好国内外人才资源，不将国（境）外学习或工作经历作为人才招聘引进的限制性条件。
        </div>
        <div class="CFRight">C float:right</div>
    </div>
</body>
```

- 接下来对 C 盒子进行相对定位，将 C 盒子位置向右侧移动 100%，并向左上方向拉回一个 C 盒子的宽高（不然会看不到哟）。这样在文本未溢出时不会看到 C 盒子，在文本溢出时，显示 C 盒子。

```css
.multiline .CFRight {
    background: yellow;
    float: right;
    width: 100px;
    height: 20px;
    position: relative;
    left: 100%;
    transform: translate(-100%, -100%);
}
```

![](http://img.up-4ever.site/20201219191852.gif)

- 最后需要做的就是文本溢出时隐藏

```css
.multiline {
    overflow: hidden;
    max-height: 60px;
    line-height: 20px;
    position: relative;
}
```

![](http://img.up-4ever.site/20201219192216.gif)

## 最后

本文介绍了几种目前常见的文本截断省略的方案，各有利弊，各位同学可根据实际开发情况及需求选择方案。如果你还知道更好其他实现方案，欢迎在评论区留下宝贵评论。

## 参考文章

- [可能是最全的 “文本溢出截断省略” 方案合集](https://www.zoo.team/article/text-overflow)
- [-webkit-line-clamp](https://developer.mozilla.org/zh-CN/docs/Web/CSS/-webkit-line-clamp)

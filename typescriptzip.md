---
title: 一道 3 层的 TypeScript 面试题，你能答到第几层？
date: '2023-02-11'
tags:
  - TypeScript
cover: 'http://img.up-4ever.site/infinity-1901105%20(1).jpg'
categories:
  - TypeScript
top_img: 'http://img.up-4ever.site/infinity-4164366.jpg'
abbrlink: 75ae98af
---
# 一道 3 层的 TypeScript 面试题，你能答到第几层？

## 前言

本文转载自公众号**神光的编程秘籍**，对文章作者**神说要有光**表示感谢~

原文链接：[https://mp.weixin.qq.com/s/wWqeF5JJO3WlmBpDIcSuiw](https://mp.weixin.qq.com/s/wWqeF5JJO3WlmBpDIcSuiw)

## 正文

最近遇见一道不错的 TS 面试题，分享一下。

这道题有 3 个层次，我们一层层来看。

第一层的要求是这样的：

**实现一个 zip 函数，对两个数组的元素按顺序两两合并，比如输入 [1,2,3], [4,5,6] 时，返回 [[1,4], [2,5],[3,6]]**

这层就是每次各从两个数组取一个元素，合并之后放到数组里，然后继续处理下一个，递归进行这个流程，直到数组为空即可。

```js
function zip(target, source) {
  if (!target.length || !source.length) {
    return [];
  }
  const [firstTargetItem, ...restTarget] = target; 
  const [firstSourceItem, ...restSource] = source;
  return [[firstTargetItem, firstSourceItem], ...zip(restTarget, restSource)];
}
```

结果是对的：

![](http://img.up-4ever.site/20230211173910.png)

第一层还是比较简单的，然后我们来看第二层要求：

**给这个 zip 函数定义 ts 类型（两种写法）**

函数的定义有两种形式：

直接通过 function 声明函数：

```js
function func() {}
```

和声明匿名函数然后赋值给变量：

```js
const func = () => {}
```

而参数和返回值的类型都是数组，只是具体类型不知道，可以写 unknown[]。

所以两种函数类型的定义就是这样的：

```typescript
function zip(target: unknown[], source: unknown[]): unknown[] {
  if (!target.length || !source.length) {
    return [];
  }
  const [firstTargetItem, ...restTarget] = target; 
  const [firstSourceItem, ...restSource] = source;
  return [[firstTargetItem, firstSourceItem], ...zip(restTarget, restSource)];
}
```

```typescript
interface Zip {
  (target: unknown[], source: unknown[]): unknown[]
}

const _zip: Zip = (target: unknown[], source: unknown[]) => {
  if (!target.length || !source.length) {
    return [];
  }
  const [firstTargetItem, ...restTarget] = target; 
  const [firstSourceItem, ...restSource] = source;
  return [[firstTargetItem, firstSourceItem], ..._zip(restTarget, restSource)];
}
```

也是直接 function 声明函数类型和 interface 声明函数类型然后加到变量类型上两种。

因为具体元素类型不知道，所以用 unknown。

这里可能会问 any 和 unknown 的区别：

any 和 unknown 都可以接收任何类型：

![](http://img.up-4ever.site/20230211175355.png)

any 也可以赋值给任何类型，但 unknown 不行。

![](http://img.up-4ever.site/20230211175519.png)

这里只是用来接收其他类型， 所以 unknown 比any 更合适一些，更安全。

这一层也是比较基础的 ts 语法，第三层就上了难度了：

**用类型编程实现精确的类型提示，比如参数传入 [1,2,3], [4,5,6]，那返回值的类型要提示出 [[1,4], [2,5],[3,6]]**

这里要求返回值类型是精确的，我们就要根据参数的类型来动态生成返回值类型。

也就是这样：

![](http://img.up-4ever.site/20230211181511.png)

声明两个类型参数 Target、Source，约束为 unknown[]，也就是元素类型任意的数组类型。

这俩类型参数分别是传入的两个参数的类型。

返回值通过 Zip 计算得出。

然后要实现 Zip 的高级类型：

传入的类型参数分别是两个数组类型，我们同样要从中提取出每个元素合并到一起。

提取元素可以用模式匹配的方式：

![](http://img.up-4ever.site/20230211182029.png)

![](http://img.up-4ever.site/20230211182129.png)

所以这个类型就可以这样定义：

```typescript
type Zip<Target extends unknown[], Source extends unknown[]> =
  Target extends [infer firstTargetItem, ...infer restTarget]
    ? Source extends [infer firstSourceItem, ...infer restSource]
      ? [[firstTargetItem, firstSourceItem], ...Zip<restTarget, restSource>]
      : []
    :[];
```

分别提取两个数组的第一个元素，构造成新数组。然后对剩下的数组递归进行这样的处理，直到数组为空。

这样就实现了我们想要的高级类型：

![](http://img.up-4ever.site/20230211183126.png)

但你把它作为返回值加到函数上会报错：

![](http://img.up-4ever.site/20230211184225.png)

因为声明函数的时候都不知道参数是啥，自然计算不出 Zip<Target, Source> 的值，所以这里会类型不匹配(如上图)。

那怎么办呢？

可以用函数重载解决：

```typescript
function zip<Target extends unknown[], Source extends unknown[]>(
  target: Target,
  source: Source,
): Zip<Target, Source>
function zip(target: unknown[], source: unknown[]): unknown[];

function zip(target: unknown[], source: unknown[]) {
  if (!target.length || !source.length) {
    return [];
  }
  const [firstTargetItem, ...restTarget] = target; 
  const [firstSourceItem, ...restSource] = source;
  return [[firstTargetItem, firstSourceItem], ...zip(restTarget, restSource)];
}
```

ts 支持函数重载，可以写多个同名函数的类型的类型定义，最后写函数的实现，这样用到这个函数的时候会根据参数的类型来匹配函数类型。

我们用了类型编程的那个函数通过这种方式写就不会报错了。

我们使用下看看：

![](http://img.up-4ever.site/20230211184835.png)

咋返回值的类型不对呢？

![](http://img.up-4ever.site/20230211184909.png)

其实这时候匹配的函数类型是对的，只不过推导出的不是字面量类型。

这时候可以加个 as const。

![](http://img.up-4ever.site/20230211185413.png)

这样类型就不匹配了，所以要在类型参数的声明上也加上 readonly:

![](http://img.up-4ever.site/20230211185645.png)

但这样 Zip 函数的类型又不匹配了。

难道要把所有用到这个类型的地方都加上 readonly 么？

不用，我们 readonly 的修饰去掉不就行了？

Typescript 有内置的高级类型 readonly：

![](http://img.up-4ever.site/20230211185757.png)

可以把索引类型的每个索引都加上 readonly 修饰：

![](http://img.up-4ever.site/20230211190003.png)

但没有提供去掉 readonly 修饰的高级类型，我们可以自己实现一下：

```typescript
type Mutable<T> = {
  -readonly [P in keyof T]: T[P];
}
```

用映射类型的语法构造个新索引类型，加上个 -readonly 就是去掉 readonly 修饰的意思。

> [TypeScript: Documentation - Mapped Types (typescriptlang.org)](https://www.typescriptlang.org/docs/handbook/2/mapped-types.html#mapping-modifiers)

![](http://img.up-4ever.site/20230211190530.png)

有的同学可能问了，数组类型也是索引类型么？

是，索引类型是聚合多个元素的类型，所以对象、数组、class 都是。

所以我们把它用在数组上自然也是可以的：

![](http://img.up-4ever.site/20230211190654.png)

（准确来说叫元组，元组是元素个数固定的数组）

那我们只要在传入 Zip 之前，用 Mutable 去掉 readonly 就可以了：

```typescript
function zip<Target extends readonly unknown[], Source extends readonly unknown[]>(
  target: Target,
  source: Source,
): Zip<Mutable<Target>, Mutable<Source>>
function zip(target: unknown[], source: unknown[]): unknown[];

function zip(target: unknown[], source: unknown[]) {
  if (!target.length || !source.length) {
    return [];
  }
  const [firstTargetItem, ...restTarget] = target; 
  const [firstSourceItem, ...restSource] = source;
  return [[firstTargetItem, firstSourceItem], ...zip(restTarget, restSource)];
}
```

再来试一下：

![](http://img.up-4ever.site/20230211190944.png)

![](http://img.up-4ever.site/20230211191008.png)

大功告成！现在返回值的类型就对了。

但还有个问题，如果传入的不是断言常量（`as const`）的参数，是推导不出字面量类型的，这时候貌似就不对了：

![](http://img.up-4ever.site/20230211191629.png)

可我们不都声明重载类型了么？

如果推导不出字面量类型，应该匹配这个呀：

![](http://img.up-4ever.site/20230211192737.png)

但实际上它匹配的还是第一个：

![](http://img.up-4ever.site/20230211192812.png)

这是因为：

![](http://img.up-4ever.site/20230211192703.png)

也就是说第一个函数类型是可以匹配到的。

这时候其实只要调换下两个函数类型的顺序就可以了：

![](http://img.up-4ever.site/20230211192946.png)

为什么呢？因为**重载函数的类型是从上到下依次匹配，只要匹配到一个就应用。**

全部代码如下：

```typescript
/**
 * 实现一个 zip 函数，对两个数组的元素按顺序两两合并，比如输入 [1,2,3], [4,5,6] 时，返回 [[1,4], [2,5],[3,6]]
*/

type Zip<Target extends unknown[], Source extends unknown[]> =
  Target extends [infer firstTargetItem, ...infer restTarget]
    ? Source extends [infer firstSourceItem, ...infer restSource]
      ? [[firstTargetItem, firstSourceItem], ...Zip<restTarget, restSource>]
      : []
    :[];

type Mutable<T> = {
  -readonly [P in keyof T]: T[P];
}

function zip(target: unknown[], source: unknown[]): unknown[];
function zip<Target extends readonly unknown[], Source extends readonly unknown[]>(
  target: Target,
  source: Source,
): Zip<Mutable<Target>, Mutable<Source>>

function zip(target: unknown[], source: unknown[]) {
  if (!target.length || !source.length) {
    return [];
  }
  const [firstTargetItem, ...restTarget] = target; 
  const [firstSourceItem, ...restSource] = source;
  return [[firstTargetItem, firstSourceItem], ...zip(restTarget, restSource)];
}

const res = zip([1,2,3] as const, [4,5,6] as const);

const targetList = [1,2,3];
const targetListConst = [1,2,3] as const;

const sourceList = [4,'5',6];
const sourceListConst = [4,'5',6] as const;

const result = zip(targetList, sourceList);
const resultConst = zip(targetListConst, sourceListConst);
```

## 总结

今天我们做了一道综合的 ts 面试题，一共有三层：

第一层实现 js 的逻辑，用递归或者循环都能实现。

第二层给函数加上类型，用 function 声明类型和 interface 声明函数类型两种方式，参数和返回值都是 unknown[]。

第三层是用类型编程实现精准的类型提示，这一层需要拿到参数的类型，通过提取元素的类型并构造出新的数组类型返回。还要通过函数重载的方式来声明类型，并且要注意重载类型的声明顺序。

as const 能够让字面量推导出字面量类型，但会带有 readonly 修饰，可以自己写映射类型来去掉这个修饰。

其实这也是我们学习 ts 的顺序，我们先要能把 js 逻辑写出来，然后知道怎么给函数、class 等加 ts 类型，之后学习类型编程，知道怎么动态生成类型。

其中**类型编程**是 ts 最难的部分，也是最强大的部分。攻克了这一层，ts 就可以说学的差不多了。
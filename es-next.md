---
title: ES next
description: ES next
sticky: 10
date: '2021-07-01'
cover: 'http://img.up-4ever.site/20210704162016.jpeg'
top_img: 'http://img.up-4ever.site/infinity-1115505.jpg'
tags:
  - JavaScript
categories:
  - JavaScript
abbrlink: e5ef1fdd
---
# ES next

## ECMAScript 历史

ECMA，读音类似“埃科妈”，是欧洲计算机制造商协会（European Computer Manufacturers Association）的简称，是一家国际性会员制度的信息和电信标准组织。1994 年之后，由于组织的标准牵涉到很多其他国家，为了体现其国际性，更名为 Ecma 国际（Ecma International），因此 Ecma 就不再是首字母缩略字了。

1995 年，著名的网景公司（Netscape）的 Brendan Eich 开发了一种脚本语言，最初命名为 Mocha，后来改名为 LiveScript，最后为了蹭当时火热的 Java 热度重命名为了 JavaScript。

了解了 Ecma 国际和 JavaScript，就方便了解 ECMAScript 了，ECMAScript 是一种由 Ecma 国际在标准 **ECMA-262** 中定义的 脚本语言 规范。这种语言在往往被称为 JavaScript 或 JScript ，但实际上 JavaScript 和 JScript 是 ECMA-262 标准的实现和扩展。

## 神秘的 ECMA-262

上文提到了第一个神秘代码 ECMA-262，ECMA-262 到底是什么呢？原来 Ecma 国际的标准，都会以 Ecma-Number 命名，ECMA-262 就是 ECMA 262 号标准，具体就是指 ECMAScript 遵照的标准。1996 年 11 月，网景公司将 JavaScript 提交给 Ecma 国际进行标准化。ECMA-262 的第一个版本于 1997 年 6 月被 Ecma 国际采纳。

尽管 JavaScript 和 JScript 与 ECMAScript 兼容，但包含超出 ECMAScript 的功能。

我们如何查看最新最全的 Ecma 标准呢，可以查看 Ecma 国际官网的 Standards。截止到 2021 年 7 月，最新的 Ecma 标准已经到了 ECMA-419。

![](http://img.up-4ever.site/20210707115052.png)

Ecma 标准涉及的类别非常多，官网因此提供了按照类别和最新修改排序的功能，我们来看看 ECMA-262 属于哪个类别：

![](http://img.up-4ever.site/20210707115453.png)

**ECMA-262 属于“软件工程与接口”类别**，该类别一共有 12 个标准，详见上图。注意，ECMA-262 的最新更新日期是 2021 年 6 月，**ES2021 正式成为 ECMA 标准**。

## 探秘 [Ecma TC39](https://www.ecma-international.org/technical-committees/tc39/) 神秘组织

TC39 是 Technical Committee 39 的简称，是制定 ECMAScript 标准的委员会，由各个主流浏览器厂商的代表构成，主席团三人分别来自 PayPal、Bloomberg 和 Microsoft，下设两个工作组（task group） TC39-TG1 和 TC39-TG2 。

TC39-TG1 工作组主要工作是通用 ECMAScript® 语言， 包括语法、语义、类库以及支持该语言的技术。

TC39-TG2 工作组 ECMAScript® 国际化 API 标准。

## ES6

上文提到 ECMAScript 是由 Ecma 国际在标准 ECMA-262 中定义的脚本语言规范。**到 2015 年，一共发布了 1、2、3、4、5、5.1、6 共 7 个版本（其中 4 被废弃）**。

我们常把 5.1 之前的 ECMAScript 版本统称做 **ES5**，将 6 版本之后的版本统称做 **ES6**（因为从 2015 年起，ECMAScript 终于步入正轨，每年发布一次版本，到了 2021 年，已经发布了 6 个版本了，实在太多，所以用变革了 JavaScript 时代的 ES6 作为后续版本的代称）。

从 ECMAScript 第 6 版开始，每年发布一个 ECMAScript 版本，因此 ECMAScript 版本有了很多名字，包括全名 ECMAScript 6、简写 ES6、年份命名 ECMAScript 2015、年份简写 ES2015。最常见的名字还是 ES6，之后推出的 ES7、ES8 等同理。

| 版本 |    版本全称    |           版本别称            |   发布日期    | 简介                                                         |
| :--: | :------------: | :---------------------------: | :-----------: | :----------------------------------------------------------- |
|  1   |  ECMAScript 1  |              ES1              |   1997年6月   | 首版                                                         |
|  2   |  ECMAScript 2  |              ES2              |   1998年6月   | 格式修正，以使得其形式与ISO/IEC16262国标一致                 |
|  3   |  ECMAScript 3  |              ES3              |  1999年12月   | 强大的正则表达式，更好的词法作用域链处理，新的控制指令，异常处理，错误定义更加明确，数据输出的格式化及其他改变 |
|  4   |  ECMAScript 4  |              ES4              |     放弃      | 由于关于语言的复杂性出现分歧，第4版本被放弃，其中的部分成为第5版本及Harmony的基础 |
|  5   |  ECMAScript 5  |              ES5              |  2009年12月   | 新增『严格模式』，一个子集用作提供更彻底的错误检查，以避免结果出错，澄清了许多第3版本的模糊规范，并适应了与规范不一致的真实世界实现的行为。增加了部分新功能，如getters及setters，支持JSON以及在对象属性上更完整的反射 |
| 5.1  | ECMAScript 5.1 |             ES5.1             |   2011年6月   | ECMAScript 5.1版形式上完全一致于国标ISO/IEC16262:2011        |
|  6   |  ECMAScript 6  | ES6、ECMAScript 2015、ES2015  |   2015年6月   | 第6版，添加了类和模块的语法，其他特性包括迭代器，生成器和生成器表达式，箭头函数，二进制数据、静态类型数数组，集合，promise，reflect和proxy。 |
|  7   |  ECMAScript 7  | ES7、ECMAScript 2016、ES2016  |   2016年6月   | 第7版，多个新的概念和语言特性                                |
|  8   |  ECMAScript 8  | ES8、ECMAScript 2017、ES2017  |   2017年6月   | 第8版，多个新的概念和语言特性                                |
|  9   |  ECMAScript 9  | ES9、ECMAScript 2018、ES2018  |   2018年6月   | 第9版，包含了异步循环，生成器，新的正则表达式特性和rest/spread语法 |
|  10  | ECMAScript 10  | ES10、ECMAScript 2019、ES2019 |   2019年6月   | 第10版                                                       |
|  11  | ECMAScript 11  | ES11、ECMAScript 2020、ES2020 |   2020年6月   | 第11版                                                       |
|  12  | ECMAScript 12  | ES12、ECMAScript 2021、ES2021 |   2021年6月   | 第12版                                                       |
|  13  | ECMAScript 13  | ES13、ECMAScript 2022、ES2022 | 预计2022年6月 | 尚未发布，已经进入stage4，预计2022年6月通过，正式成为ECMA标准 |

需要注意的是，自从 TC39 流程 制定以来，ECMAScript 版本的重要性就降低了很多。大家不必记住某一个 ES 特性到底是哪年推出的。现在真正重要的是提案处于哪个阶段：一旦提案到了第 4 阶段，那么它就可以使用了。但是即使这样，你仍然需要检查你的引擎是否支持该功能。

这里又提到了一个 TC39 流程 和 阶段（Stage）的概念，我们接下来看看这两个概念是什么含义。

## [TC39 流程](https://exploringjs.com/impatient-js/ch_history.html#tc39-process) 和 Stage X

- 如果两个版本之间经过了太多的时间，那么早已准备好的特性就必须等待很长时间才可以发布。而且功能准备如果很晚，会增加 deadline 之前匆忙赶工的风险。
- 很多功能在其实现和使用之前就花了很长时间在设计上。因此，发现与实现和使用相关的设计缺陷会非常晚。

为了解决上述问题， TC39 建立了新的 TC39 流程：

- ECMAScript 功能设计与每年的 ECMAScript 版本发布独立，使用不同阶段（Stage）来区分功能的状态，共 5 个阶段，从 Stage 0（strawman）开始，到 Stage 4 （finished）结束。
- 越往后的阶段，需要原型实现和真机测试，可以建立设计和实现之间的反馈机制。
- ECMAScript 版本每年发布一次，发布的内容包含在 release deadline 之前的全部到达 Stage 4 的功能。

![](http://img.up-4ever.site/20210707163730.png)

ES2016是第一个根据TC39流程设计的ECMAScript版本。

## 官方ECMAscript新特性列表

2016年到目前 (2021-07-07)，进入stage4阶段的新特性有41个：

具体可参考[Finished Proposal](https://github.com/tc39/proposals/blob/master/finished-proposals.md).

ES2015（ES6）新增的特性比较多，不方便在此一一列举。可参考阮一峰老师的《ES6标准入门（第三版）》（以下简称《ES6》）。

### New in ECMAScript 2016

ECMAScript 2016中添加了以下新特性：

- [Array.prototype.includes()](https://github.com/tc39/proposal-Array.prototype.includes)
- [Exponentiation Operator](https://github.com/tc39/proposal-exponentiation-operator)（**）

### New in ECMAScript 2017

ECMAScript 2017中添加了以下新特性：

- [Object.values(), Object.entries()](https://github.com/tc39/proposal-object-values-entries)
- String padding: [String.prototype.padStart / String.prototype.padEnd](https://github.com/tc39/proposal-string-pad-start-end)
- [Object.getOwnPropertyDescriptors()](https://github.com/tc39/proposal-object-getownpropertydescriptors)

> 注意Object.getOwnPropertyDescriptor(obj, key) 和 Object.getOwnPropertyDescriptors(obj) 的区别

- [Proposal to allow trailing commas in function parameter lists and calls](https://github.com/tc39/proposal-trailing-function-commas)『函数参数列表允许使用尾随逗号』

```js
function clownPuppiesEverywhere(
  param1,
  param2, // Next parameter that's added only has to add a new line, not modify this line
 ) { /* ... */ } 

clownPuppiesEverywhere(
  'foo',
  'bar', // Next parameter that's added only has to add a new line, not modify this line
);
```

- [Async functions](https://github.com/tc39/proposal-async-await)
- Shared memory and atomics『共享内存和Atomics对象』

> 可参考之前的[文章](http://up-4ever.site/posts/bc5b4b9/#SharedArrayBuffer)

### New in ECMAScript 2018

- Template literal revision『模板文字修订』，可参考阮一峰老师《ES6》CH4-4.14 模板字符串的限制 P68
- [`s` (`dotAll`) flag for regular expressions](https://github.com/tc39/proposal-regexp-dotall-flag)『s修饰符：dotAll模式』，可参考阮一峰老师《ES6》CH5-5.7 s修饰符：dotAll模式 P78
- [RegExp named capture groups](https://github.com/tc39/proposal-regexp-named-groups)『正则表达式命名捕获组以及后向引用』可参考之前的文章[正则表达式30分钟入门教程](http://up-4ever.site/posts/522e21a3/#%E5%90%8E%E5%90%91%E5%BC%95%E7%94%A8)

对于任何标识符名称，可以使用 `(?<name>...)` 语法为捕获组命名。

```js
let re = /(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/u;
let result = re.exec('2015-01-02');
// result.groups.year === '2015';
// result.groups.month === '01';
// result.groups.day === '02';

// result[0] === '2015-01-02';
// result[1] === '2015';
// result[2] === '01';
// result[3] === '02';

// 支持解构赋值
let {groups: {one, two}} = /^(?<one>.*):(?<two>.*)$/u.exec('foo:bar');
console.log(`one: ${one}, two: ${two}`);  // prints one: foo, two: bar
```

后向引用：

命名组可以通过`\k<name>` 结构在正则表达式中引用。 例如，

```js
let duplicate = /^(?<half>.*).\k<half>$/u;
duplicate.test('a*b'); // false
duplicate.test('a*a'); // true
```

命名引用与编号引用可以同时使用。

```js
let triplicate = /^(?<part>.*).\k<part>.\1$/u;
triplicate.test('a*a*a'); // true
triplicate.test('a*a*b'); // false
```

- [Object Rest/Spread Properties](https://github.com/tc39/proposal-object-rest-spread)
- [RegExp Lookbehind Assertions](https://github.com/tc39/proposal-regexp-lookbehind)『正则表达式的后向断言』
- [RegExp Unicode Property Escapes](https://github.com/tc39/proposal-regexp-unicode-property-escapes)『正则表达式 Unicode属性转义』可参考阮一峰老师《ES6》CH5-5.3 u修饰符
- [Promise.prototype.finally](https://github.com/tc39/proposal-promise-finally)
- [Asynchronous Iteration](https://github.com/tc39/proposal-async-iteration)『异步迭代器』可参考阮一峰老师《ES6》CH18-18.7 异步遍历器

### New in ECMAScript 2019

- [Optional `catch` binding](https://github.com/tc39/proposal-optional-catch-binding)『可选的捕获绑定』

此提案引入的语法更改允许省略 catch 绑定及其周围的括号，如

```js
try {
  // ...
} catch {
  // ...
}
```

- [JSON superset](https://github.com/tc39/proposal-json-superset)
- [`Symbol.prototype.description`](https://github.com/tc39/proposal-Symbol-description)

```js
// The parameter we pass to the symbol factory function provides a description for the created symbol:
const mySymbol = Symbol('mySymbol');
// The description can be accessed in two ways.
// First, it is part of the string returned by .toString():
assert.equal(mySymbol.toString(), 'Symbol(mySymbol)');
// Second, since ES2019, we can retrieve the description via the property .description:
assert.equal(mySymbol.description, 'mySymbol');

//注意与Symbol.for() 和 Symbol.keyFor() 的区别
const for1 = Symbol.for('a');
const for2 = Symbol.for('a');
assert.equal(Symbol.keyFor(for1), 'a');
assert.equal(for1 === for2, true);
```

- [`Function.prototype.toString` revision](https://2ality.com/2016/08/function-prototype-tostring.html)

> 摘抄两段话：
>
> The ECMAScript proposal “[`Function.prototype.toString` revision](https://tc39.github.io/Function-prototype-toString-revision/)” (by Michael Ficarra) is at [stage 4](http://exploringjs.com/es2016-es2017/ch_tc39-process.html) and therefore part of ECMAScript 2019. It brings two major improvements [compared to ES2016](https://tc39.github.io/ecma262/2016/#sec-function.prototype.tostring):
>
> - Whenever possible – source code: If a function was created via ECMAScript source code, `toString()` must return that source code. In ES2016, whether to do so is left up to engines.
> - Otherwise – standardized placeholder: In ES2016, if `toString()` could not (or would not) create syntactically valid ECMAScript code, it had to return a string for which `eval()` throws a `SyntaxError`. In other words, `eval()` must not be able to parse the string. This requirement was forward-incompatible – whatever string you come up with, you can never be completely sure that a future version of ECMAScript doesn’t make it syntactically valid. In contrast, the proposal standardizes a placeholder: a function whose body is `{ [native code] }`. 

- [`Object.fromEntries`](https://github.com/tc39/proposal-object-from-entries)

`Object.fromEntries` 会执行 Object.entries 的相反操作：它接受可迭代的键值对并返回一个新对象，其键和相应值由键值对给出。

```js
obj = Object.fromEntries([['a', 0], ['b', 1]]); // { a: 0, b: 1 }
```

把对象转为Map：

```js
obj = { foo: true, bar: false };
map = new Map(Object.entries(obj));
```

然而，从却没有`Object.entries`的逆操作，使得可以从键值对构造对象，因此通常必须编写一个辅助程序：

```js
obj = Array.from(map).reduce((acc, [key, value]) => Object.assign(acc, { [key]: value }), {});
```

在有了`Object.fromEntries`之后，我们可以这样：

```js
obj = Object.fromEntries(map);
```

对于其他类似 Map 的对象，这种转换可能也很简单：

```js
query = Object.fromEntries(new URLSearchParams('foo=bar&baz=qux'));
```

> 关于[URLSearchParams](https://developer.mozilla.org/zh-CN/docs/Web/API/URLSearchParams)

对于其他集合，中间转换可以将集合转换为所需的形式：

```js
arr = [ { name: 'Alice', age: 40 }, { name: 'Bob', age: 36 } ];
obj = Object.fromEntries(arr.map(({ name, age }) => [ name, age ]));
```

更多例子用法可参考[Object.formEntries()-Motivating examples](https://github.com/tc39/proposal-object-from-entries#motivating-examples)

- [Well-formed `JSON.stringify`](https://github.com/tc39/proposal-well-formed-stringify)『格式良好的JSON.stringify』
- [`String.prototype.{trimStart,trimEnd}`](https://github.com/tc39/proposal-string-left-right-trim)

```js
'    a b c    '.trim(); // "a b c"

'    a b c    '.trimStart(); // "a b c    "
'    a b c    '.trimLeft(); // "a b c    "

'    a b c    '.trimEnd(); // "    a b c"
'    a b c    '.trimRight(); // "    a b c"
```

> 为了与padStart/padEnd保持一致，trimStart和trimEnd被纳为标准函数，但是为了web兼容性，`trimLeft` 将别名为 `trimStart`，`trimRight` 将别名为 `trimEnd`。这意味着在大多数引擎中，`String.prototype.trimRight.name` 将从“trimRight”更改为“trimEnd”。

```js
String.prototype.trimRight.name === "trimEnd"; // true
```

- [`Array.prototype.{flat,flatMap}`](https://github.com/tc39/proposal-flatMap)

```js
// Array.prototype.flat(depth = 1): any[]

[ 1,2, [3,4], [[5,6]] ].flat(0); // [ 1, 2, [3,4], [[5,6]] ]
[ 1,2, [3,4], [[5,6]] ].flat(1); // [1, 2, 3, 4, [5,6]]
[ 1,2, [3,4], [[5,6]] ].flat(2); // [1, 2, 3, 4, 5, 6]

// Array.prototype.flatMap(
//	callback: (value: T, index: number, array: T[]) => U | U[],
//	thisValue?: any
//): U[]

['a', 'b', 'c'].flatMap(x => [x, x]); // ["a", "a", "b", "b", "c", "c"]
['a', 'b', 'c'].flatMap(x => [x]); // ["a", "b", "c"]
['a', 'b', 'c'].flatMap(x => []); // []
['a', 'b', 'c'].flatMap(x => x.repeat(3)); // ["aaa", "bbb", "ccc"]
['a', 'b', 'c'].flatMap(x => [x, [x]]); // ["a", Array(1), "b", Array(1), "c", Array(1)]
```

### New in ECMAScript 2020

- [`String.prototype.matchAll`](https://github.com/tc39/proposal-string-matchall)

`.matchAll()`的调用方法：

```js
const matchIterable = str.matchAll(regExp);
```

给定一个字符串和一个正则表达式，` .matchAll()` 返回一个包含所有匹配对象的**迭代**。

```js
[...'-a-a-a'.matchAll(/-(a)/ug)];
// 输出为：
[
  { 0:'-a', 1:'a', index: 0, input: '-a-a-a', groups: undefined },
  { 0:'-a', 1:'a', index: 2, input: '-a-a-a', groups: undefined },
  { 0:'-a', 1:'a', index: 4, input: '-a-a-a', groups: undefined },
]
```

> 标志位`/g`是必须的

```js
[...'-a-a-a'.matchAll(/-(a)/u)]
// TypeError: String.prototype.matchAll called with a non-global RegExp argument
```

`.matchAll()`不受 `regExp.lastIndex`的影响并且也不会改变这个值。

我们自己也可以实现一个`.matchAll()`:

```js
function* matchAll(str, regExp) {
  if (!regExp.global) {
    throw new TypeError('Flag /g must be set!');
  }
  const localCopy = new RegExp(regExp, regExp.flags);
  let match;
  while (match = localCopy.exec(str)) {
    yield match;
  }
}
```

- Dynamic imports via[`import()`](https://github.com/tc39/proposal-dynamic-import)

到目前位置，引入模块的唯一方式是通过`import`表达式。其有几条限制如下：

	1. 必须在模块的顶层使用。也就是说，不可以在块内导入某些内容。
	2. 模块标识符总是固定的。也就是说，不能根据条件更改导入的内容。而且不能动态地组装模块的标识符。

`import()`改变了这一现状。举个栗子：

现有文件如下：

​	lib/my-math.mjs

​	main1.mjs

​	main2.mjs

其中my-math.mjs内容如下：

```js
function times(a, b) {
  return a * b;
}
export function square(x) {
  return times(x, x);
}
export const LIGHTSPEED = 299792458;
```

那么，我们在main1.mjs中可以以下面这种方式来引入：

```js
const dir = './lib/';
const moduleSpecifier = dir + 'my-math.mjs';

function loadConstant() {
  return import(moduleSpecifier)
  .then(myMath => {
    const result = myMath.LIGHTSPEED;
    assert.equal(result, 299792458);
    return result;
  });
}
```

注意，在这段代码中有两处是之前所不允许的：

1. 在函数内导入模块（不是模块顶层）
2. 模块的标识符是一个变量

在main2.mjs中，我们可以通过`async`函数来实现同样的功能：

```js
const dir = './lib/';
const moduleSpecifier = dir + 'my-math.mjs';

async function loadConstant() {
  const myMath = await import(moduleSpecifier);
  const result = myMath.LIGHTSPEED;
  assert.equal(result, 299792458);
  return result;
}
```

**import() 的使用场景：**

​	case1：*在需要时加载代码*

```js
button.addEventListener('click', event => {
  import('./dialogBox.mjs')
    .then(dialogBox => {
      dialogBox.open();
    })
    .catch(error => {
      /* Error handling */
    })
});
```

​	case2：*模块的条件加载*

```js
if (isLegacyPlatform()) {
  import('./my-polyfill.mjs')
    .then(···);
}
```

​	case3：*Computed module specifiers*

```js
import(`messages_${getLocale()}.mjs`)
  .then(···);
```

- [`BigInt`](https://github.com/tc39/proposal-bigint)『github文档很详细』

- [`Promise.allSettled`](https://github.com/tc39/proposal-promise-allSettled)

该`Promise.allSettled()`方法返回一个在所有给定的promise都已经`fulfilled`**或**`rejected`后的promise，并带有一个对象数组，每个对象表示对应的promise结果。

当您有多个彼此不依赖的异步任务成功完成时，或者您总是想知道每个`promise`的结果时，通常使用它。

相比之下，`Promise.all()` 更适合彼此相互依赖或者在其中任何一个`reject`时立即结束。

```js
const promise1 = Promise.resolve(3);
const promise2 = new Promise((resolve, reject) => setTimeout(reject, 100, 'foo'));
const promises = [promise1, promise2];
(async () => {
  const data = await Promise.allSettled(promises);
  data.forEach((item) => {
    console.log(item);
  })
})();

//output:
// "fulfilled"
// "rejected"
```

- [`globalThis`](https://github.com/tc39/proposal-global)
- [`for-in` mechanics](https://github.com/tc39/proposal-for-in-order)『for-in 机制』
- [Optional Chaining](https://github.com/tc39/proposal-optional-chaining)『可选链』（`?.`）

可选链存在一下几种情况中：

```
obj?.prop     // optional static property access
obj?.[«expr»] // optional dynamic property access
func?.(«arg0», «arg1») // optional function or method call
```

如果在问号前面的值既不是 `undefined` 也不是 `null`，那么就会执行问号的运算。否则，返回 `undefined`。

举个栗子：

```js
const persons = [
  {
    surname: 'Zoe',
    address: {
      street: {
        name: 'Sesame Street',
        number: '123',
      },
    },
  },
  {
    surname: 'Mariner',
  },
  {
    surname: 'Carmen',
    address: {
    },
  },
];

// 我们可以用可选链比较安全的提取街道名称

const streetNames = persons.map(
  p => p.address?.street?.name);
assert.deepEqual(
  streetNames, ['Sesame Street', undefined, undefined]
);
```

- [Nullish coalescing Operator](https://github.com/tc39/proposal-nullish-coalescing)『空值合并运算符`??`』

**空值合并操作符**（**`??`**）是一个逻辑操作符，当左侧的操作数为 [`null`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null) 或者 [`undefined`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/undefined) 时，返回其右侧操作数，否则返回左侧操作数。

下面的两个表达式是等效的：

```js
a ?? b
a !== undefined && a !== null ? a : b
```

上面可选链的例子，可以结合空值合并运算符，来使用默认值『'no street'』来代替 `undefined`：

```js
const streetNames = persons.map(
  p => p.address?.street?.name ?? 'no street name');
assert.deepEqual(
  streetNames, ['Sesame Street', 'no street name', 'no street name']
);
```

- [`import.meta`](https://github.com/tc39/proposal-import-meta)

### New in ECMAScript 2021

- [`String.prototype.replaceAll`](https://github.com/tc39/proposal-string-replaceall)

```js
let str = 'abc abf adabfasdadfa';
console.log(str.replace('a', '*'));// *bc abf adabfasdadfa
console.log(str.replace(/a/, '*'));// *bc abf adabfasdadfa
console.log(str.replace(/a/g, '*'));// *bc *bf *d*bf*sd*df*

console.log(str.replaceAll('a', '*'));// *bc *bf *d*bf*sd*df*
// console.log(str.replaceAll(/a/, '*')); 报错，replaceAll 当使用一个 `regex`时，您必须设置全局（“ g”）标志
console.log(str.replaceAll(/a/g, '*'));// *bc *bf *d*bf*sd*df*
```

> 当使用一个 `regex`时，您必须设置全局（“ g”）标志，否则，它将引发 `TypeError`：“必须使用全局 RegExp 调用 replaceAll”。

- [`Promise.any`](https://github.com/tc39/proposal-promise-any)

`Promise.any()` 接收一个[`Promise`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)可迭代对象，只要其中的一个 `promise` 成功，就返回那个已经成功的 `promise` 。如果可迭代对象中没有一个 `promise` 成功（即所有的 `promises` 都失败/拒绝），就返回一个失败的 `promise `和[`AggregateError`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/AggregateError)类型的实例，它是 [`Error`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Error) 的一个子类，用于把单一的错误集合在一起。本质上，这个方法和[`Promise.all()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)是相反的。

```js
(async () => {
    const data = await Promise.any([
      new Promise((resolve, reject) => {
        setTimeout(() => {
          reject('2 second');
        }, 2000)
      }),
      new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve('5 second');
        }, 5000)
      })
    ]);
    console.log(data);
 })();

// output:
// 5 second
```

与Promise.race()的区别：

- Promise.any(): 以第一个**成功** (resolve) 为准，失败不管
- Promie.race(): 以第一个**完成** (迭代器中的某个promise成功『resolve』或失败『reject』) 的为准

```js
(async () => {
    const data = await Promise.race([
      new Promise((resolve, reject) => {
        setTimeout(() => {
          reject('2 second');
        }, 2000)
      }),
      new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve('5 second');
        }, 5000)
      })
    ]);
    console.log(data);
})();

// output:
// Uncaught (in promise) 2 second
```

- [WeakRefs](https://github.com/tc39/proposal-weakrefs)

之前存在弱引用：[weakMap](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)、[weakSet](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakSet)

WeakRef对象包含对对象的弱引用，这个弱引用被称为该WeakRef对象的target或者是referent。对对象的弱引用是指当该对象应该被GC回收时不会阻止GC的回收行为。而与此相反的，一个普通的引用（默认是强引用）会将与之对应的对象保存在内存中。只有当该对象没有任何的强引用时，JavaScript引擎GC才会销毁该对象并且回收该对象所占的内存空间。如果上述情况发生了，那么你就无法通过任何的弱引用来获取该对象。

- 实例方法

[`WeakRef.prototype.deref()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/WeakRef/deref): 返回当前实例的WeakRef对象所绑定的target对象，如果该target对象已被GC回收则返回`undefined`。

```js
class Counter {
  constructor(element) {
    // Remember a weak reference to the DOM element
    this.ref = new WeakRef(element);
    this.start();
  }

  start() {
    if (this.timer) {
      return;
    }

    this.count = 0;

    const tick = () => {
      // Get the element from the weak reference, if it still exists
      const element = this.ref.deref();
      if (element) {
        element.textContent = ++this.count;
      } else {
        // The element doesn't exist anymore
        console.log("The element is gone.");
        this.stop();
        this.ref = null;
      }
    };

    tick();
    this.timer = setInterval(tick, 1000);
  }

  stop() {
    if (this.timer) {
      clearInterval(this.timer);
      this.timer = 0;
    }
  }
}

const counter = new Counter(document.getElementById("counter"));
counter.start();
setTimeout(() => {
  document.getElementById("counter").remove();
}, 5000);
```

- [Logical Assignment Operators](https://github.com/tc39/proposal-logical-assignment)『逻辑赋值运算符』

之前主要存在的复合赋值运算符：+=、-=、*=、/=、%=、<<=、>>=、>>>=

新版本中增加了逻辑赋值运算符：&&=、||=、??=

```js
const books = [
  {
    isbn: '123',
  },
  {
    title: 'ECMAScript Language Specification',
    isbn: '456',
  },
];

// Add property .title where it’s missing
for (const book of books) {
  book.title ??= '(Untitled)';
}

assert.deepEqual(
  books,
  [
    {
      isbn: '123',
      title: '(Untitled)',
    },
    {
      title: 'ECMAScript Language Specification',
      isbn: '456',
    },
  ]);
```

- [Numeric separators](https://github.com/tc39/proposal-numeric-separator)『数字分隔符』

```js
let a = 10_000_000_0000_0;
console.log(a);// 1000000000000
```

> 主要是便于阅读，书写比较长的数字。

### New in ECMAScript 2022

- Class Fields ([Private instance methods and accessors](https://github.com/tc39/proposal-private-methods), [Class Public Instance Fields & Private Instance Fields](https://github.com/tc39/proposal-class-fields), [Static class fields and private static methods](https://github.com/tc39/proposal-static-class-features))

- [RegExp Match Indices](https://github.com/tc39/proposal-regexp-match-indices)

- [Top-level `await`](https://github.com/tc39/proposal-top-level-await)

- [Ergonomic brand checks for Private Fields](https://github.com/tc39/proposal-private-fields-in-in)

- [`.at()`](https://github.com/tc39/proposal-relative-indexing-method)

  `.at()` 提案建议在 `Array`, `String`, and `TypedArray` 增加一个实例方法 `.at()`, 接受一个整数作为参数，返回在整数项的元素。

  - 原因

    ![](http://img.up-4ever.site/20221001162931.png)

  - 例子

    ```js
    const arr = [1, 2, 3, 4, 5];
    // 取最后一个元素
    // 没有 .at() 方法之前
    const lastArrItem = arr[arr.length - 1];
    // 用 .at() 方法
    const lastArrItemAt = arr.at(-1);
    // 字符串同理
    const str = 'hello, world!';
    const lastChart = str.at(-2);
    ```

  - Polyfill

    ```js
    function at(n) {
    	// ToInteger() abstract op
    	n = Math.trunc(n) || 0;
    	// Allow negative indexing from the end
    	if (n < 0) n += this.length;
    	// OOB access is guaranteed to return undefined
    	if (n < 0 || n >= this.length) return undefined;
    	// Otherwise, this is just normal property access
    	return this[n];
    }
    
    const TypedArray = Reflect.getPrototypeOf(Int8Array);
    for (const C of [Array, String, TypedArray]) {
        Object.defineProperty(C.prototype, "at",
                              { value: at,
                                writable: true,
                                enumerable: false,
                                configurable: true });
    }
    ```

- [Accessible `Object.prototype.hasOwnProperty`](https://github.com/tc39/proposal-accessible-object-hasownproperty)

  > Proposal for an `Object.hasOwn()` method to make `Object.prototype.hasOwnProperty()` more accessible.
  >
  > This proposal adds a `Object.hasOwn(object, property)` method with the same behavior as calling `hasOwnProperty.call(object, property)`
  >
  > ```js
  > let object = { foo: false }
  > Object.hasOwn(object, "foo") // true
  > 
  > let object2 = Object.create({ foo: true })
  > Object.hasOwn(object2, "foo") // false
  > 
  > let object3 = Object.create(null)
  > Object.hasOwn(object3, "foo") // false
  > ```

  - 原因

    如今，下面的代码（尤其是在各种三方库中）是非常常见的：

    ```js
    let hasOwnProperty = Object.prototype.hasOwnProperty
    
    if (hasOwnProperty.call(object, "foo")) {
      console.log("has property foo")
    }
    ```

    此提案目的是简化上面的代码：

    ```js
    if (Object.hasOwn(object, "foo")) {
      console.log("has property foo")
    }
    ```

    其他原因详见[README.md]([GitHub - tc39/proposal-accessible-object-hasownproperty: Object.hasOwn() proposal for ECMAScript](https://github.com/tc39/proposal-accessible-object-hasownproperty))

  - [Class Static Block](https://github.com/tc39/proposal-class-static-block)

  - [Error Cause](https://github.com/tc39/proposal-error-cause)

    捕获错误并将其与其他上下文数据一起抛出是错误处理模式的常见方法。有多种方法可用于将其他上下文信息追加到捕获的错误：

    ```js
    async function doJob() {
      const rawResource = await fetch('//domain/resource-a')
        .catch(err => {
          // How to wrap the error properly?
          // 1. throw new Error('Download raw resource failed: ' + err.message);
          // 2. const wrapErr = new Error('Download raw resource failed');
          //    wrapErr.cause = err;
          //    throw wrapErr;
          // 3. class CustomError extends Error {
          //      constructor(msg, cause) {
          //        super(msg);
          //        this.cause = cause;
          //      }
          //    }
          //    throw new CustomError('Download raw resource failed', err);
        })
      const jobResult = doComputationalHeavyJob(rawResource);
      await fetch('//domain/upload', { method: 'POST', body: jobResult });
    }
    
    await doJob(); // => TypeError: Failed to fetch
    ```

    如果错误与初五的原因关联到一起，那么这对于诊断意外的异常非常有帮助。如上面的示例所示，对于一个简单的错误处理案例，要用上下文消息补充捕获到的错误，需要做大量的工作。

    提案建议的解决方案是使用一个属性向构造函数添加一个额外的选项参数，该属性的值将作为属性分配给错误实例。因此，可以将错误和错误原因关联起来。

    ```js
    async function doJob() {
      const rawResource = await fetch('//domain/resource-a')
        .catch(err => {
          throw new Error('Download raw resource failed', { cause: err });
        });
      const jobResult = doComputationalHeavyJob(rawResource);
      await fetch('//domain/upload', { method: 'POST', body: jobResult })
        .catch(err => {
          throw new Error('Upload job result failed', { cause: err });
        });
    }
    
    try {
      await doJob();
    } catch (e) {
      console.log(e);
      console.log('Caused by', e.cause);
    }
    // Error: Upload job result failed
    // Caused by TypeError: Failed to fetch
    ```

    > MDN [Error() constructor]([Error() constructor - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error/Error))

  ### New in ECMAScript 2023

  - [Array find from last](https://github.com/tc39/proposal-array-find-from-last)

    Proposal for `.findLast()` and `.findLastIndex()` methods on array and typed array.

    - Core feature

      This would behave the same as [Array.prototype.find](https://www.ecma-international.org/ecma-262/11.0/index.html#sec-array.prototype.find) and [Array.prototype.findIndex](https://www.ecma-international.org/ecma-262/11.0/index.html#sec-array.prototype.findindex) but would iterate from the last to the first.
  
      ```js
      const array = [{ value: 1 }, { value: 2 }, { value: 3 }, { value: 4 }];
      
      array.find(n => n.value % 2 === 1); // { value: 1 }
      array.findIndex(n => n.value % 2 === 1); // 0
      
      // ======== Before the proposal =========== 
      
      // find
      [...array].reverse().find(n => n.value % 2 === 1); // { value: 3 }
      
      // findIndex
      array.length - 1 - [...array].reverse().findIndex(n => n.value % 2 === 1); // 2
      array.length - 1 - [...array].reverse().findIndex(n => n.value === 42); // should be -1, but 4
      
      // ======== In the proposal =========== 
      // find
      array.findLast(n => n.value % 2 === 1); // { value: 3 }
      
      // findIndex
      array.findLastIndex(n => n.value % 2 === 1); // 2
      array.findLastIndex(n => n.value === 42); // -1
      ```

  - [Hashbang Grammar](https://github.com/tc39/proposal-hashbang)

    Unix 的命令行脚本都支持`#!`命令，又称为 Hashbang。这个命令放在脚本的第一行，用来指定脚本的执行器。Hashbang Grammar 提案就是想为JavaScript 脚本引入了#!命令，这个命令写在脚本文件或者模块文件的第一行：
  
    ```js
    #!/usr/bin/env node
    // in the Script Goal
    'use strict';
    console.log(1);
    ```
  
    ```js
    #!/usr/bin/env node
    // in the Module Goal
    export {};
    console.log(1);
    ```

    这样，Unix 命令行就可以直接执行脚本了：
  
    ```shell
    # 以前执行脚本
    node hello.js
    
    # 有了 hashbang 之后执行脚本
    ./hello.js
    ```
  
  - [Symbols as WeakMap keys](https://github.com/tc39/proposal-symbols-as-weakmap-keys)
  
  - [Change Array by Copy](https://github.com/tc39/proposal-change-array-by-copy)
  
    Provides additional methods on `Array.prototype` and `TypedArray.prototype` to enable changes on the array by **returning a new copy** of it with the change.
  
    > 在 `Array.prototype` 和 `TypedArray.prototype` 上增加几个额外的方法，这些方法可以返回包含更改的新数组来完成对数组的更改。
  
    This proposal introduces the following function properties to `Array.prototype`:
  
    - `Array.prototype.toReversed() -> Array`
    - `Array.prototype.toSorted(compareFn) -> Array`
    - `Array.prototype.toSpliced(start, deleteCount, ...items) -> Array`
    - `Array.prototype.with(index, value) -> Array`
  
    All of those methods keep the target Array untouched and returns a copy of it with the change performed instead.
  
    `toReversed`, `toSorted`, and `with` will also be added to TypedArrays:
  
    - `TypedArray.prototype.toReversed() -> TypedArray`
    - `TypedArray.prototype.toSorted(compareFn) -> TypedArray`
    - `TypedArray.prototype.with(index, value) -> TypedArray`
  
    比如：
  
    ```javascript
    const sequence = [1, 2, 3];
    sequence.toReversed(); // => [3, 2, 1]
    sequence; // => [1, 2, 3]
    
    const outOfOrder = new Uint8Array([3, 1, 2]);
    outOfOrder.toSorted(); // => Uint8Array [1, 2, 3]
    outOfOrder; // => Uint8Array [3, 1, 2]
    
    const correctionNeeded = [1, 1, 3];
    correctionNeeded.with(1, 5); // => [1, 5, 3]
    correctionNeeded; // => [1, 1, 3]
    ```
  
  ### New in ECMAScript 2024
  
  - [Well-Formed Unicode Strings](https://github.com/tc39/proposal-is-usv-string)
  
  - [`Atomics.waitAsync`](https://github.com/tc39/proposal-atomics-wait-async)
  
  - [Array Grouping](https://github.com/tc39/proposal-array-grouping)
  
    A proposal to make grouping of items in an array (and iterables) easier.
  
    类似于：[`_.groupBy`](https://lodash.com/docs/4.17.15#groupBy) ([850k downloads/week](https://www.npmjs.com/package/lodash.groupby))
  
    提供了两个静态方法 `Object.groupBy` and  `Map.groupBy` 。第一个方法返回一个原型为空的对象，这样做的原因是：方便解构并防止与全局对象属性发生意外冲突。 第二个返回一个常规 Map 实例，它允许对复杂键类型进行分组。
  
    至于为什是静态方法（而不是实例方法）提案人员给出了答复：
  
    ![image-20231209203337052](http://img.up-4ever.site/20231209203338.png)
  
    最后我们一起看一下例子：
  
    ```javascript
    const array = [1, 2, 3, 4, 5];
    
    // `Object.groupBy` groups items by arbitrary key.
    // In this case, we're grouping by even/odd keys
    Object.groupBy(array, (num, index) => {
      return num % 2 === 0 ? 'even': 'odd';
    });
    // =>  { odd: [1, 3, 5], even: [2, 4] }
    
    // `Map.groupBy` returns items in a Map, and is useful for grouping
    // using an object key.
    const odd  = { odd: true };
    const even = { even: true };
    Map.groupBy(array, (num, index) => {
      return num % 2 === 0 ? even: odd;
    });
    // =>  Map { {odd: true}: [1, 3, 5], {even: true}: [2, 4] }
    ```
  
    - [`Promise.withResolvers`](https://github.com/tc39/proposal-promise-with-resolvers)
  
      这个提案的具体原因我不提了，可以去点开链接看一下，个人认为是非常非常使用的一个提案了，目前已经 stage 3 了~

## 最后

本文会持续更新，更新依据主要是根据[Finished Proposal](https://github.com/tc39/proposals/blob/master/finished-proposals.md)发布的提案。

## 更新

- 2021.07.19更新[Ergonomic brand checks for Private Fields](https://github.com/tc39/proposal-private-fields-in-in)
- 2022.10.01更新
  - [`.at()`](https://github.com/tc39/proposal-relative-indexing-method)
  - [Accessible `Object.prototype.hasOwnProperty`](https://github.com/tc39/proposal-accessible-object-hasownproperty)
  - [Class Static Block](https://github.com/tc39/proposal-class-static-block)
  - [Error Cause](https://github.com/tc39/proposal-error-cause)
  - [Array find from last](https://github.com/tc39/proposal-array-find-from-last)
  - [Hashbang Grammar](https://github.com/tc39/proposal-hashbang)
- 2023.12.09
  - [Symbols as WeakMap keys](https://github.com/tc39/proposal-symbols-as-weakmap-keys)
  - [Change Array by Copy](https://github.com/tc39/proposal-change-array-by-copy)
  - [Well-Formed Unicode Strings](https://github.com/tc39/proposal-is-usv-string)
  - [`Atomics.waitAsync`](https://github.com/tc39/proposal-atomics-wait-async)
  - [Array Grouping](https://github.com/tc39/proposal-array-grouping)
  - [`Promise.withResolvers`](https://github.com/tc39/proposal-promise-with-resolvers)
  


## 参考文章

- [New JavaScript features](https://exploringjs.com/impatient-js/ch_new-javascript-features.html)
- [全网最全 ECMAScript 攻略](https://blog.csdn.net/FrontendRadioo/article/details/116059097)
- [Finished Proposal](https://github.com/tc39/proposals/blob/master/finished-proposals.md)
- [The TC39 Process](https://tc39.es/process-document/)
- [ES2019: Function.prototype.toString revision](https://2ality.com/2016/08/function-prototype-tostring.html)

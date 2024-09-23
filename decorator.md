---
title: TypeScript 装饰器完全指南
date: '2023-03-01'
tags:
  - TypeScript
categories:
  - TypeScript
cover: 'http://img.up-4ever.site/infinity-265049.jpg'
top_img: 'http://img.up-4ever.site/infinity-6462079.jpg'
abbrlink: 50f6df40
---

# TypeScript 装饰器完全指南

> 本文大多部分参考[A Complete Guide to TypeScript Decorators](https://mirone.me/zh-hans/a-complete-guide-to-typescript-decorator/),对作者表示感谢~

## 概览

装饰器本质上是一种特殊的函数被应用于:

- 类
- 类属性
- 类方法
- 类访问器
- 类方法的参数

所以装饰器其实很像是在组合一系列函数，类似于高阶函数和类。通过装饰器我们可以轻松实现代理模式使代码更简洁以及实现一些更有趣的能力。

装饰器的语法十分简单，只需要在想使用的装饰器前面加上`@`符号，装饰器就会被应用到目标上：

```ts
function decorator() {
  console.log('I am a decorator');
}

@decorator
class A {}
```

装饰器的类型一共可以分为：

1. 类装饰器
2. 属性装饰器
3. 方法装饰器
4. 访问器装饰器
5. 参数装饰器

让我们快速认识一下这 5 种装饰器

```ts
// 类装饰器
@classDecorator
class Bird {
  // 属性装饰器
  @propertyDecorator
  name: string;
  
  // 方法装饰器
  @functionDecorator
  getName(
    // 参数装饰器
    @paramsDecorator
    nickName: string
  ) {
    return `${this.name}(${nickName})`;
  }

  // 访问器装饰器
  @accessorDecorator
  get bridName() {
    return this.name;
  }
}
```

装饰器的写法可分为：普通装饰器（无法传参） 和 装饰器工厂（可以传参）

```ts
// 普通装饰器
function normalDecorator(target: any) {
  console.log(target);
};

@normalDecorator
class Person {}

// 装饰器工厂
function decoratorFactory(params: string) {
  return function (target: any) {
    console.log(target, params);
  };
};

@decoratorFactory('params')
class Bird {}
```

## 执行

### 执行时机

装饰器只在解释执行时被应用一次，例如：

```ts
function f(C) {
  console.log('apply decorator')
  return C
}

@f
class A {}

// output: apply decorator
```

在终端中会打印`apply decorator`，尽管我们没有使用`A`类。

### 执行顺序

不同类型的装饰器的执行顺序是有明确定义的：

1. 首先是实例成员, 参数装饰器 => 方法/访问器/属性 装饰器；
2. 其次是静态成员, 参数装饰器 => 方法/访问器/属性 装饰器；
3. 然后是应用在构造函数上的参数装饰器；
4. 最后是应用在类上的类装饰器；

例如，考虑一下代码：

```ts
function f(key: string): any {
  console.log("evaluate: ", key);
  return function () {
    console.log("call: ", key);
  };
}

@f("Class Decorator")
class C {
  @f("Static Property")
  static prop?: number;

  @f("Static Method")
  static method(@f("Static Method Parameter") foo: any) {}

  constructor(@f("Constructor Parameter") foo: any) {}

  @f("Instance Method")
  method(@f("Instance Method Parameter") foo: any) {}

  @f("Instance Property")
  prop?: number;
}
```

以上代码将在控制台打印如下信息：

```plainText
evaluate:  Instance Method
evaluate:  Instance Method Parameter
call:  Instance Method Parameter
call:  Instance Method
evaluate:  Instance Property
call:  Instance Property
evaluate:  Static Property
call:  Static Property
evaluate:  Static Method
evaluate:  Static Method Parameter
call:  Static Method Parameter
call:  Static Method
evaluate:  Class Decorator
evaluate:  Constructor Parameter
call:  Constructor Parameter
call:  Class Decorator
```

你也许会注意到实例属性`prop`晚于实例方法`method`，但是静态属性`prop`早于静态方法`method`，这是因为对于属性/方法/访问器装饰器而言，执行顺序取决于它们的声明顺序。

然而，同一方法中不同参数的装饰器的执行顺序是相反的， 最后一个参数的装饰器会最先被执行：

```ts
function f(key: string): any {
  console.log("evaluate: ", key);
  return function () {
    console.log("call: ", key);
  };
}

class C {
  method(
    @f("Parameter Foo") foo,
    @f("Parameter Bar") bar
  ) {}
}
```

上面代码在控制台会打印出：

```plainText
evaluate:  Parameter Foo
evaluate:  Parameter Bar
call:  Parameter Bar
call:  Parameter Foo
```

### 多个装饰器的组合

你可以对同一目标应用多个装饰器，它们的组合顺序为：

1. 求值外层装饰器
2. 求值内层装饰器
3. 调用内层装饰器
4. 调用外层装饰器

例如：

```ts
function f(key: string) {
  console.log("evaluate: ", key);
  return function () {
    console.log("call: ", key);
  };
}

class C {
  @f("Outer Method")
  @f("Inner Method")
  method() {}
}
```

以上代码将会在控制台打印出：

```plainText
evaluate: Outer Method
evaluate: Inner Method
call: Inner Method
call: Outer Method
```

## 定义

### 类装饰器

类型声明：

```ts
type ClassDecorator = <TFunction extends Function>(target: TFunction) => TFunction | void;
```

- 参数

  1. `target`: 类的构造函数

  > class A {}; A.prototype.constructor === A // true

- 返回：如果类装饰器返回了一个值，它将会被用来代替原有的类声明；

**因此，类装饰器适合用于继承一个现有类并添加一些属性和方法。**

例如我们可以添加一个`toString`方法给所有的类来覆盖它原有的`toString`方法。

```ts
type Consturctor = { new (...args: any[]): any };

function toString<T extends Consturctor>(BaseClass: T) {
  return class extends BaseClass {
    toString() {
      return JSON.stringify(this);
    }
  };
}

@toString
class C {
  public foo = "foo";
  public num = 24;
}

console.log(new C().toString());
// {"foo":"foo","num":24}
```

遗憾的是装饰器并没有类型保护，这意味着：

```ts
function reportableClassDecorator<T extends { new (...args: any[]): any }>(constructor: T) {
  return class extends constructor {
    reportingURL = "http://www...";
  };
}

@reportableClassDecorator
class BugReport {
  type = "report";
  title: string;

  constructor(t: string) {
    this.title = t;
  }
}

const bug = new BugReport("Needs dark mode");
console.log(bug.title); // Prints "Needs dark mode"
console.log(bug.type); // Prints "report"

// Note that the decorator _does not_ change the TypeScript type
// and so the new property `reportingURL` is not known
// to the type system:
bug.reportingURL; // Property 'reportingURL' does not exist on type 'BugReport'.
```

这是一个[TypeScript的已知的缺陷](https://github.com/microsoft/TypeScript/issues/4881)。 目前我们能做的只有额外提供一个类用于提供类型信息：

```ts
function reportableClassDecorator<T extends { new (...args: any[]): any }>(constructor: T) {
  return class extends constructor {
    reportingURL = "http://www...";
  };
}

class Base {
  reportingURL!: string;
}

@reportableClassDecorator
class BugReport extends Base{
  type = "report";
  title: string;

  constructor(t: string) {
    super();
    this.title = t;
  }
}

const bug = new BugReport("Needs dark mode");
console.log(bug.title); // Prints "Needs dark mode"
console.log(bug.type); // Prints "report"

bug.reportingURL;
```

### 属性装饰器

类型声明：

```ts
type PropertyDecorator = (target: Object, propertyKey: string | symbol) => void;
```

- 参数
  1. `target`: 对于静态成员来说是类的构造函数，对于实例成员来说是类的原型；
  2. `propertyKey`：属性名称

- 返回：返回值将被忽略。

除了用于收集信息外，属性装饰器也可以用来给类添加额外的方法和属性。 例如我们可以写一个装饰器来给某些属性添加监听器。

```ts
function capitalizeFirstLetter(str: string) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

function observable(target: any, key: string): any {
  // prop -> onPropChange
  const targetKey = "on" + capitalizeFirstLetter(key) + "Change";

  target[targetKey] =
    function (fn: (prev: any, next: any) => void) {
      let prev = this[key];
      Reflect.defineProperty(this, key, {
        set(next) {
          fn(prev, next);
          prev = next;
        }
      })
    };
}

class C {
  @observable
  foo = -1;

  @observable
  bar = "bar";
}

const c = new C();

c.onFooChange((prev, next) => console.log(`prev: ${prev}, next: ${next}`))
c.onBarChange((prev, next) => console.log(`prev: ${prev}, next: ${next}`))

c.foo = 100; // -> prev: -1, next: 100
c.foo = -3.14; // -> prev: 100, next: -3.14
c.bar = "baz"; // -> prev: bar, next: baz
c.bar = "sing"; // -> prev: baz, next: sing
```

### 方法装饰器

类型声明：

```ts
type MethodDecorator = <T>(
  target: Object, propertyKey: string | symbol, descriptor: TypedPropertyDescriptor<T>
) => TypedPropertyDescriptor<T> | void;
```

- 参数
  1. `target`: 对于静态成员来说是类的构造函数，对于实例成员来说是类的原型；
  2. `propertyKey`：属性名称
  3. `descriptor`: 属性[描述器](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)

- 返回：如果返回了值，它会被用于替代属性的描述器。

方法装饰器不同于属性装饰器的地方在于`descriptor`参数。 通过这个参数我们可以修改方法原本的实现，添加一些共用逻辑。 例如我们可以给一些方法添加打印输入与输出的能力：

```ts
function logger(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const original = descriptor.value;

  descriptor.value = function (...args) {
    console.log('params: ', ...args);
    const result = original.call(this, ...args);
    console.log('result: ', result);
    return result;
  }
}

class C {
  @logger
  add(x: number, y:number ) {
    return x + y;
  }
}

const c = new C();
c.add(1, 2);
// -> params: 1, 2
// -> result: 3
```

### 访问器装饰器

访问器装饰器总体上讲和方法装饰器很接近，唯一的区别在于描述器中有的key不同：

方法装饰器的描述器的key为：

- configurable
- enumerable
- writable
- value

访问器装饰器的key为：

- configurable
- enumerable
- get
- set

例如，我们可以将某个属性设为不可变值：

```ts
function immutable(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
  const original = descriptor.set;

  descriptor.set = function (value: any) {
    return original.call(this, { ...value })
  }
}

class C {
  private _point = { x: 0, y: 0 }

  @immutable
  set point(value: { x: number, y: number }) {
    this._point = value;
  }

  get point() {
    return this._point;
  }
}

const c = new C();
const point = { x: 1, y: 1 }
c.point = point;

console.log(c.point === point)
// -> false
```

### 参数装饰器

类型声明：

```ts
type ParameterDecorator = (target: Object, propertyKey: string | symbol, parameterIndex: number) => void;
```

- 参数
  1. `target`: 对于静态成员来说是类的构造函数，对于实例成员来说是类的原型；
  2. `propertyKey`：属性名称(注意是**方法**的名称，而不是参数的名称)
  3. `parameterIndex`: 参数在方法参数列表中所处位置的下标

- 返回：返回值会被忽略

单独的参数装饰器能做的事情很有限，它一般都被用于记录可被其它装饰器使用的信息。

## 结合

对于一些复杂场景， 我们可能需要结合使用不同的装饰器。 例如如果我们不仅想给我们的接口添加静态检查，还想加上运行时检查的能力。

我们可以用3个步骤来实现这个功能：

1. 标记需要检查的参数 (因为参数装饰器先于方法装饰器执行)。
2. 改变方法的`descriptor`的`value`的值，先运行参数检查器，如果失败就抛出异常。
3. 运行原有的接口实现。

以下是代码:

```ts
type Validator = (x: any) => boolean;

// save the marks
const validateMap: Record<string, Validator[]> = {};

// 1. 标记需要检查的参数
function typedDecoratorFactory(validator: Validator): ParameterDecorator {
  return (_, key, index) => {
    const target = validateMap[key as string] ?? [];
    target[index] = validator;
    validateMap[key as string] = target;
  }
}

function validate(_: Object, key: string, descriptor: PropertyDescriptor) {
  const originalFn = descriptor.value;
  descriptor.value = function(...args: any[]) {

    // 2. 运行检查器
    const validatorList = validateMap[key];
    if (validatorList) {
      args.forEach((arg, index) => {
        const validator = validatorList[index];

        if (!validator) return;

        const result = validator(arg);

        if (!result) {
          throw new Error(
            `Failed for parameter: ${arg} of the index: ${index}`
          );
        }
      });
    }

    // 3. 运行原有的方法
    return originalFn.call(this, ...args);
  }
}

const isInt = typedDecoratorFactory((x) => Number.isInteger(x));
const isString = typedDecoratorFactory((x) => typeof x === 'string');

class C {
  @validate
  sayRepeat(@isString word: string, @isInt x: number) {
    return Array(x).fill(word).join('');
  }
}

const c = new C();
c.sayRepeat('hello', 2); // pass
c.sayRepeat('', 'lol' as any); // throw an error
```

正如例子中展示的， 对我们来说同时理解不同种类装饰器的执行顺序和职责都很重要。

## 元数据

严格地说，元数据和装饰器是EcmaScript中两个独立的部分。 然而，如果你想实现像是[反射](https://zh.wikipedia.org/wiki/%E5%8F%8D%E5%B0%84_(%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%A7%91%E5%AD%A6))这样的能力，你总是同时需要它们。

如果我们回顾上一个例子，如果我们不想写各种不同的检查器呢？ 或者说，能否只写一个检查器能够通过我们编写的TS类型声明来自动运行类型检查？

有了[reflect-metadata](https://github.com/rbuckton/reflect-metadata)的帮助， 我们可以获取编译期的类型。

```ts
import 'reflect-metadata';

function validate(
  target: any,
  key: string,
  descriptor: PropertyDescriptor
) {
  const originalFn = descriptor.value;

  // 获取参数的编译期类型
  const designParamTypes = Reflect
    .getMetadata('design:paramtypes', target, key);

  descriptor.value = function (...args: any[]) {
    args.forEach((arg, index) => {

      const paramType = designParamTypes[index];

      const result = arg.constructor === paramType
        || arg instanceof paramType;

      if (!result) {
        throw new Error(
          `Failed for validating parameter: ${arg} of the index: ${index}`
        );
      }
    });

    return originalFn.call(this, ...args);
  }
}

@Reflect.metadata('type', 'class')
class C {
  @validate
  sayRepeat(word: string, x: number) {
    return Array(x).fill(word).join('');
  }
}

const c = new C();
c.sayRepeat('hello', 2); // pass
c.sayRepeat('', 'lol' as any); // throw an error
```

以上代码[Playground](https://www.typescriptlang.org/play?strictPropertyInitialization=false&target=1&jsx=4#code/PTAEAEFMFsEsBcAikDGB7ATgQ3pgspPFgCY5YBQIEkAHgA6QazSQB2RANsutrhgM6Uw4fvCYp4ABQxoGGeAE8AkqwSwsHWAC8csNKwBcoAGYb+kcszqZ4oAEQZIxjqngBaFkVJE7AbnLkxgCurBJ6rKAAbhqw3pAAFOSgoEQYAOaERgDyAEYAVq4ANEmgANaQCkaiTKxpxcnEkPwoTHR8RtKyjIrIza185ACUoADeJeisoqCYsGmwrBoAYhEAvKCNfbBtmAB00RxBkP4lVIDtwYBryoBDyoAOpoAhboBo-oCL0YD45oDePoDR6uP6UxuzrJJYbDQAAqCgY-FAawASk4XBISskdhl4AQvGR4gByX5pQx0QFYaCKcEYwopQHI0nlBSDY7JCb8NAuHYcNBpeLY-74kFgpo0gINJotLZ8PYaQ6QkwhML6UDxHby8n8IxYVgKADaAF1hmNkslFTtjJgAKJYFAAC3i8XJpPmjRowxWAD5RvzdXTvrY8UDQQwJRyAd6efw1bbaBraW7QPTbI5+EEOLY1uSdtGMEEJJhISs1l6CT6LJHdQAfIugcmgeaiFUoSBoYygXPchgRt2wevxACEsfj8G1CMj8DNMgA7qBWJBR0aMDIMIlC26AAaLLCwFzEEyZ-axXS1BtcwiMIwAEhG5IAvtN64PIBXWHbjyNQzQzwv+26+YWz-2z3z+454EEGARDMcwLBwywphoHDxIOsD8KS8o7IqH6gF+X7kCgHBYPwEIAMIusk4BbnEJT8FgCgwgwODxMOmDEFUYjzHUoA0EYrBBNAOSMH2br-oBEQAILTuR8T2gaq7QbRGDEIMOx5Gg8yYhiKFoQE0ZRhK46jrh8R8igOxkRRkBUfAmJmpAHAsiSoAAEw0qAVB4jhGEGeRlGQNRNllhCKoKKSACMAAM9lUIOI5lhEjAzkAA)

目前为止一共有三种编译期类型可以拿到：

- `design:type`: 属性的类型。
- `desin:paramtypes`: 方法的参数的类型。
- `design:returntype`: 方法的返回值的类型。

这三种方式拿到的结果都是构造函数（例如`String`和`Number`）。规则是：

- number -> `Number`
- string -> `String`
- boolean -> `Boolean`
- void/null/never -> `undefined`
- Array/Tuple -> `Array`
- Class -> 类的构造函数
- Enum -> 如果是纯数字枚举则为`Number`, 否则是 `Object`
- Function -> `Function`
- 其余都是`Object`

## 何时使用

现在我们可以对于何时使用装饰器得出结论， 在阅读上面的代码中你可能也有所感觉。

我将例举一些常用的使用场景：

- Before/After钩子
- 监听属性改变或者方法调用
- 对方法的参数做转换
- 添加额外的方法和属性
- 运行时类型检查
- 自动编解码
- 依赖注入

## 参考文章

- [TypeScript-decorator](https://www.typescriptlang.org/docs/handbook/decorators.html)
- [A Complete Guide to TypeScript Decorators](https://mirone.me/a-complete-guide-to-typescript-decorator/)
- [TS系列篇｜装饰器(@)](https://juejin.cn/post/7006483808832716813)

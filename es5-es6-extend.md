---
title: ES5/6对象的继承及区别
date: '2020-06-18'
tags:
  - 前端开发
  - JavaScript
categories:
  - JavaScript
cover: 'http://img.up-4ever.site/20201017105512.jpeg'
top_img: 'http://img.up-4ever.site/infinity-1115505.jpg'
abbrlink: 6b00b46
---
# ES5/6对象的继承

## ES5的对象继承(组合继承)

```javascript
 function user(name, age) {
    this.name = name;
    this.age = age;
  }

  user.prototype.sayName = function () {
    console.log(this.name);
  }

  function vipuser(name, age, level) {
    user.call(this, name, age);
    this.level = level;
  }

  vipuser.prototype = new user();
  vipuser.prototype.constructor = vipuser;

  vipuser.prototype.saylevel = function () {
    console.log(this.level);
  }

  console.log(Reflect.ownKeys(vipuser.prototype))
  console.log(Object.keys(vipuser.prototype))


  let vip = new vipuser('tom', 12, 6);
  vip.saylevel(); //6
  vip.sayName(); //tom
```

## ES6的对象继承

```javascript
class user {
    constructor(name, age) {
      this.name = name;
      this.age = age;
    }

    sayName() {
      console.log(this.name);
    }
  }

  class vipuser extends user {
    constructor(name, age, level) {
      super(name, age); //调用父类的constructor(name, age)
      this.level = level;
    }

    saylevel() {
      console.log(this.level);
    }
  }

  console.log(Reflect.ownKeys(vipuser.prototype))
  console.log(Object.keys(vipuser.prototype))

  let vip = new vipuser('tom', 12, 6);
  vip.saylevel(); //6
  vip.sayName(); //tom
```

构造函数的prototype属性在ES6的“类”上依然存在。事实上，类的所有方法都定义在类的prototype属性上。

## 区别以及要注意的点

### 区别1：属性的枚举性

  类内部定义的所有方法都是不可枚举的。
  
  ```javascript
  //ES5
  console.log(Object.keys(vipuser.prototype))
  //["name", "age", "constructor", "saylevel"]
  console.log(Reflect.ownKeys(vipuser.prototype);
  //["name", "age", "constructor", "saylevel"]
  ```

  ```javascript
  //ES6
  console.log(Reflect.ownKeys(vipuser.prototype));
  //["constructor", "saylevel"]
  console.log(Object.keys(vipuser.prototype));
  //[]
  ```

### 区别2：类必须使用new来调用

  那如果要把ES5的构造函数也必须使用new，应该如何改造？

  ```javascript
  function F() {
    if(!(this instanceof F)) throw new Error('need new keyword');
    ...
  }

  //或者

  function F(){
    if(!new.target) throw new Error("need new keyword");
    console.log(new.target);
  }
  ```

### 区别3：类不存在变量提升

  类不存在变量提升，这一点与ES5完全不一样。

  ```javascript
  new Foo(); //ReferenceError
  class Foo {}
  ```
  
  这种规定的原因与继承有关，必须要保证子类在父类之后定义。

  ```javascript
  {
    let Foo = class {};
    class Bar extends Foo {}
  }
  ```

  这段代码不会报错，因为Bar继承Foo的时候，Foo已经定义了。但是如果存在变量提升，上面的代码就会报错。因为class会被提升到顶部，而let命令是不提升的，所以导致Bar继承Foo的时候，Foo还没有定义。

### 区别4：继承机制区别

  ES5的继承实质是先创造子类的实例对象，然后再将父类的方法添加到this上面(Parent.apply(this))。ES6的继承机制完全不同，实质是先创造父类的实例对象this(所以必须先调通super方法)，然后再用子类的构造函数去修改this。

- 注意点
  
  子类必须在constructor中调用super方法，否则新建实例时会报错。在子类的构造函数中，只有调用super之后才可以使用this关键字，否则报错。


## 属性的遍历(外加补充)

  - for...in..

    for...in...循环遍历对象**自身的**和**继承的*****可枚举***属性(不含Symbol属性)

  - Object.keys(obj)

    Object.keys(obj)返回一个数组，包含对象**自身的**(不含继承)所有 ***可枚举*** 的属性(不含Symbol属性)

  - Object.getOwnPropertyNames(obj)

    Object.getOwnPropertyNames(obj)返回一个数组，包含对象自身的**所有**属性(不含Symbol属性，但是包含不可枚举属性)

  - Object.getOwnPropertySymbols(obj)

    Object.getOwnPropertySymbols(obj)返回一个数组，包含对象自身的**所有**Symbol属性。

  - Reflect.ownKeys(obj)

    Reflect.ownKeys(obj)返回一个数组，包含对象**自身**的所有属性，不管属性名是Symbol还是字符串，也不管是否可枚举。

    以上5种遍历对象属性的方法都遵守同样的遍历顺序：

    1. 首先遍历所有属性名为数值的属性，按照数字排序.
    2. 其次遍历所有属性名为字符串的属性，按照生成时间排序.
    3. 最后遍历所有属性名为Symbol的属性，按照生成时间排序.

    ```javascript
    Reflect.ownKeys({
      [Symbol()]: 0,
      b: 0,
      10: 0,
      2: 0,
      a: 0
    })
    //['2', '10', 'b', 'a', Symbol()]
    ```

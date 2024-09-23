---
title: TypeScript 装饰器之依赖注入
date: '2023-04-05'
tags:
  - TypeScript
categories:
  - TypeScript
cover: 'http://img.up-4ever.site/infinity-12769278.jpg'
top_img: 'http://img.up-4ever.site/infinity-6462079.jpg'
abbrlink: e823e90e
---
# TypeScript 装饰器之依赖注入

> **墙裂建议**先看上一篇文章：[TypeScript 装饰器完全指南](http://up-4ever.site/posts/50f6df40/)

## 什么是依赖注入

依赖注入是将一个对象所依赖的其他对象直接提供给这个对象，而不是在当前对象中直接实例化这些依赖的对象。

在[Dependency Injection Demystified](https://www.jamesshore.com/v2/blog/2006/dependency-injection-demystified)一文中的解释更是让人啧啧称赞：

> **Dependency injection means giving an object its instance variables. Really. That’s it.**
> “Dependency Injection” is a 25-dollar term for a 5-cent concept. That’s not to say that it’s a bad term... and it’s a good tool. But the top articles on Google focus on bells and whistles at the expense of the basic concept. I figured I should say something, well, simpler.

## 为什么要依赖注入

考虑下面这种场景：

我们先定义四个Class，车，车身，底盘，轮胎。然后初始化这辆车，最后跑这辆车。

![](http://img.up-4ever.site/20230415213404.jpg)

假设我们需要改动一下轮胎（Tire）类，把它的尺寸变成动态的，而不是一直都是30。

![](http://img.up-4ever.site/20230415213426.jpg)

由于我们修改了轮胎的定义，为了让整个程序正常运行，我们需要做以下改动：

![](http://img.up-4ever.site/20230415213444.jpg)

由此我们可以看到，仅仅是为了修改轮胎的构造函数，这种设计却需要修改整个上层所有类的构造函数！在软件工程中，这样的设计几乎是不可维护的。

所以我们需要进行控制反转（IoC），即上层控制下层，而不是下层控制着上层。我们用**依赖注入（Dependency Injection）** 这种方式来实现控制反转。所谓依赖注入，就是把**底层类作为参数传入上层类**，实现上层类对下层类的“控制”。

![](http://img.up-4ever.site/20230415213506.jpg)

这里我只需要修改轮胎类就行了，不用修改其他任何上层类。这显然是更容易维护的代码。并且依赖注入还有利于：

- 便于单元测试
- 解耦，统一管理被依赖对象的实例化，不用在类的内部创建被依赖对象

## 环境配置

```json
// tsconfig.json
{
  "compilerOptions": {
    //...
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    //...
  }
}
```

## 基于TS装饰器和 Reflect-metadata 实现依赖注入

借鉴 [midway](https://midwayjs.org/) 中的依赖注入实现方式，开始完成我们的基于TS装饰器和 Reflect-metadata的依赖注入。

### 框架设计

1. 我们需要用一个Map来存储注册的依赖，并且它的key必须唯一，所以我们首先设计一个容器`Container`类。
2. 注册依赖的时候尽可能简单，甚至不需要用户自己定义key，所以这里使用 Symbol 和唯一字符串来确定一个依赖。

### Container

先来设计一个`Container`类，它包括`ContainerMap`、`set`、`get`、`has`属性或方法。`ContainerMap`用来存储注册的模块，set和get用来注册和读取模块，`has`用来判断模块是否已经注册。

- `set` 形参 id 表示模块 id， value 表示模块。
- `get` 用于返回指定模块 id 对应的模块。
- `has` 用于判断模块是否注册。

```ts
class Container {
  private containerMap = new Map<string | symbol, any>();

  public set(id: string | symbol, value: any): void {
    this.containerMap.set(id, value);
  }

  public get<T extends any>(id: string | symbol): T {
    return this.containerMap.get(id) as T;
  }

  public has(id: string | symbol): boolean {
    return this.containerMap.has(id);
  }
};

const containerInstance = new Container();

export default containerInstance;
```

### Provider 类装饰器

现在实现 `Provider` 类实现注册类:

```ts
import 'reflect-metadata';
import containerInstance from "../container";

/**
 * Provider 类装饰器，注册类
 * @params {string} id
 * @params {boolean} singleton - 是否单例初始化
 * @params {any[]} 类实例化参数
*/
export function Provider(
  id?: string,
  singleton?: boolean,
  ...args: any[]
): ClassDecorator {
  return (target: any) => {
    let _id;
    let _singletonInstance;

    // 判断如果设置id， id是否唯一
    if (id && containerInstance.get(id)) {
      throw new Error(`[Service]: 此标识符(${id})已被注册使用`);
    }

    _id = id || Symbol(target.name);

    Reflect.defineMetadata('cus:id', _id, target);
    Reflect.defineMetadata('cus:args', args, target);
    Reflect.defineMetadata('cus:singleton', Boolean(singleton), target);

    if(singleton) {
      _singletonInstance = new target(...args);
    }

    containerInstance.set(_id, _singletonInstance || target);
  }
}
```

### Inject 属性装饰器

`Inject` 属性装饰器是完成将完成注册的类实例注入到容器类中。

```ts
import 'reflect-metadata';
import containerInstance from "../container";

export function Inject(id?: string): PropertyDecorator {
  return (target: Object, propertyKey: string | symbol) => {
    const dependency = Reflect.getMetadata('design:type', target, propertyKey);

    const _isSingleton: boolean = Reflect.getMetadata('cus:singleton', dependency);
    const _args: any[] = Reflect.getMetadata('cus:args', dependency) ?? [];
    const _id = id || Reflect.getMetadata('cus:id', dependency);
    
    const _dep: any = containerInstance.get(_id);

    if (!_dep) throw new Error(`[Error in Inject]: Container 中未注册 ${id}`);

    Reflect.defineProperty(target, propertyKey, {
      value: _isSingleton ? _dep : new _dep(..._args),
    });
  }
}
```

### 使用依赖注入

还是以最开始的例子为例，修改底层类是否会对上层类造成影响；

![](http://img.up-4ever.site/20230415213527.jpg)

可以看到类与类之间是解耦的，无需修改上层类来适配下层类的修改。

完整的代码如下：

```ts
// Car.ts
import { FrameWork } from "./FrameWork";
import { Inject } from "../decorator/injectDecorator";
import { Tire } from "./Tire";
export class Car {
  @Inject()
  private frameWork: FrameWork;

  @Inject('Tire')
  private tire: Tire;

  run() {
    console.log('car class run method', this.frameWork);
    console.log(this.tire.size);
  }
}

/***********************************************************/

// FrameWork.ts
import { Bottom } from "./Bottom";
import { Provider } from "../decorator/provider";
import { Inject } from "../decorator/injectDecorator";

@Provider()
export class FrameWork {
  @Inject("Bottom")
  private bottom: Bottom;
}

/***********************************************************/

// Bottom.ts
import { Tire } from "./Tire"
import { Provider } from "../decorator/provider";
import { Inject } from "../decorator/injectDecorator";

@Provider("Bottom")
export class Bottom {
  @Inject("Tire")
  private tire: Tire;
}

/***********************************************************/

// Tire.ts
import { Provider } from "../decorator/provider";

const size = 600;

@Provider("Tire", false, size) // 带参数初始化
export class Tire {
  public size: number;
  public count: number;

  constructor(size: number) {
    this.size = size;
  }
}
```

## Controller 与 Get 的实现（基于 Decorator）

上面我们基于 TS 装饰器和 Reflect-metadata 实现了一个简单的依赖注入。在 midway 中还有几个非常常用的装饰器，比如：Controller、Get、Post等，下面我们尝试实现下这几个装饰器，并完成Controller的自动扫描。最后我们通过本地的koa服务来验证结果是否符合预期。

### 记录路由信息

我们需要通过 Controller、Get、Post 装饰器把当前的路由信息通过 `Reflect-metadata` 记录下来，以方便后续我们将这些路由信息绑定在koa或者express框架上。

> 借鉴 midway 的经验，Controller 装饰器中应该记录路由的前缀，默认值是''
> Get、Post 装饰器中记录路由 path、以及路由动作

下面我们来完成 Controller、Get、Post 装饰器：

```ts
export const METHOD_METADATA = 'method';
export const PATH_PREFIX_METADATA = 'path_prefix';
export const PATH_METADATA = 'path';
// Controller
// 记录路由前缀信息
export const Controller = (prefix: string = ''): ClassDecorator => target => {
  Reflect.defineMetadata(PATH_PREFIX_METADATA, prefix, target);
};
```

```ts
// 记录路由path信息、以及相应的路由动作
const createResquestMethod = (method: 'GET' | 'POST') =>
  (path: string): MethodDecorator =>
    (target: Object, propertyKey: string | symbol, descriptor: PropertyDescriptor) =>
      {
        Reflect.defineMetadata(PATH_METADATA, path, descriptor.value);
        Reflect.defineMetadata(METHOD_METADATA, method, descriptor.value);
      }

export const Get = createResquestMethod('GET');
export const Post = createResquestMethod('POST');
```

### 处理路由信息

上一步中我们把路由的信息做了记录，接下来我们需要一个函数，这个函数的输入为某个构造函数，输出为这个构造函数的路由信息（前缀、path、路由动作等）。

```ts
import 'reflect-metadata';
import { PATH_PREFIX_METADATA, PATH_METADATA, METHOD_METADATA } from '../decorator/controllerDecorator';

export type RouteInfo = {
  path: string;
  method: 'GET' | 'POST',
  fn: Function,
  classFn: new () => any,
  methodName: string,
};

export const isConstrucor = (fn: any): boolean => {
  try {
    new fn();
  } catch (err) {
    if (err.message.indexOf('is not a constructor') >= 0) {
      return false;
    }
  }
  return true;
};

export const isFunction = (functionToCheck): boolean => {
  return functionToCheck && Object.prototype.toString.call(functionToCheck) === '[object Function]';
};

// 获取并格式化处理路由信息
export const mapRoute = (classFn: new () => any): {
  prefix: string,
  info: RouteInfo[]
} => {
  // 取出定义的 metadata 信息： 前缀
  const prefix = Reflect.getMetadata(PATH_PREFIX_METADATA, classFn);
  const methodNames = Object.getOwnPropertyNames(classFn.prototype)
    .filter(item => isFunction(classFn.prototype[item]) && !isConstrucor(classFn.prototype[item]));
  const info: RouteInfo[] = methodNames.map(methodName => {
    // 路由动作
    const fn: Function = classFn.prototype[methodName];
    // 取出定义的 metadata 信息： path
    const path: string = Reflect.getMetadata(PATH_METADATA, fn);
    // 取出定义的 metadata 信息： method
    const method: 'GET' | 'POST' = Reflect.getMetadata(METHOD_METADATA, fn);
    return {
      path,
      method,
      fn,
      classFn,
      methodName,
    };
  });
  return { prefix, info }
}
```

### 启动 Koa 服务

到目前为止我们可以通过 mapRoute 方法拿到类上我们自定义的一些 metadata，包含了路由信息。下面我们需要把路由信息给到Koa，来验证我们的路由信息是否正确。

我们现在准备一个Controller和一个Service：

**Service:**

```ts
import { Provider } from "../decorator/provider";

@Provider()
export class WebService {
  getListById() {
    return [
      {
        name: 'test',
        age: 18,
      },
      {
        name: 'te2st',
        age: 13,
      },
    ];
  }
};
```

**Controller:**

```ts
// WebController.ts
import { Config } from '../decorator/configDecorator';
import { Provider } from '../decorator/provider';
import { Controller, Get, Post } from '../decorator/controllerDecorator';
import { Inject } from '../decorator/injectDecorator';
import { WebService } from '../service/webService';

@Provider()
@Controller('/web')
export class WebController {
  @Config('token')
  token;

  @Inject()
  webService: WebService;

  @Get('/list')
  getList() {
    const data = this.webService.getListById();
    return {
      success: true,
      msg: 'ok',
      data,
      token: this.token,
    };
  }
  
  @Post('/update')
  updateListInfo() {}
}
```

设想下符合预期的场景：当本地服务启动，我们在浏览器访问 localhost:port/web/list 的时候应该可以正常返回接口数据。

**Koa 本地服务：**

```ts
import Koa from 'koa';
import Router from 'koa-router';
import 'reflect-metadata';
import { mapRoute } from "../utils/common";
import { WebController } from '../controller/webController';

const app = new Koa();
const router = new Router();

const { prefix, info } = mapRoute(WebController);

info.forEach(item => {
  const { path, method, fn, classFn } = item;
  router[method.toLowerCase()](`${prefix}${path}`, (ctx, next) => {
    ctx.body = fn.bind(classFn.prototype)();
  })
});

app.use(router.routes());

app.listen(3000, () => {
  console.log('service is running...');
});
```

### 自动扫描

其实在上一步我们的目的已基本达成，但是想一个场景：每次你新增 Controller 的时候都要到本地服务这个文件来引入这个 Controller 文件，在调用 mapRoute 方法获取路由信息，很麻烦。因此我们可以在本地服务启动的时候，自动扫描 Controller 文件夹，获取到所有的具备Provider装饰器的Controller类，然后获取类上的路由信息。

```ts
import Koa from 'koa';
import Router from 'koa-router';
import 'reflect-metadata';
import { globSync } from 'glob';
import { mapRoute, isConstrucor } from "../utils/common";

// 自动扫描 Controller
const getRouterInfoList = () => {
  // 获取 Controller 文件绝对路径
  const controllerList = globSync('**/controller/*.ts', {
    absolute: true,
  });

  // 读取 Controller 文件，格式为：Record<string, { new (): any }>[]
  const classFnList: Record<string, { new (): any }>[] = controllerList.map(controller => require(controller));

  const routeInfoList = classFnList.map(classFn => {
    return Reflect.ownKeys(classFn)
      .filter(key => key !== '__esModule')
      .map((className: string) => {
        const _class = classFn[className];
        // 确定类是被 Provider 类装饰器装饰的
        const id = Reflect.getMetadata('cus:id', _class);
        const isCons = isConstrucor(_class);
        if(isCons && id) {
          return mapRoute(_class);
        };
    });
  }).flat().filter(Boolean);
  return routeInfoList;
}

const routeInfoList = getRouterInfoList();
const app = new Koa();
const router = new Router();

routeInfoList.forEach(({ prefix, info }) => {
  info.forEach(item => {
    const { path, method, fn, classFn } = item;
    router[method.toLowerCase()](`${prefix}${path}`, (ctx, next) => {
      ctx.body = fn.bind(classFn.prototype)();
    })
  });
});

app.use(router.routes());

app.listen(3000, () => {
  console.log('service is running...');
});
```

到此我们就完成了Controller、Get、Post装饰器，并且通过本地服务验证其正确性。

## 总结

- 了解了什么是装饰器；
  - 装饰器的分类
  - 装饰器的执行时机
  - 装饰器执行顺序
- 装饰器的应用
  - 依赖注入
  - midway 中的 Controller、Get、Post装饰器的实现
  - koa本地服务验证
  - 自动扫描 Controller

## 最后

JavaScript 构建的应用不总是 OOP 的，依赖注入（控制反转）衍生于 Java 等传统技术生态，在开发习惯上存在差异。高效合理地使用 IOC 需要较大的学习成本和开发经验。但是这篇文章希望可以给大家带来一些思路和启发。

## 参考文章

- [Dependency Injection Demystified](https://www.jamesshore.com/v2/blog/2006/dependency-injection-demystified)
- [用TypeScript装饰器实现一个简单的依赖注入](https://juejin.cn/post/6872238443334729735#heading-3)
- [基于 Typescript 和 Decorator 实现依赖注入](https://developer.aliyun.com/article/721189)

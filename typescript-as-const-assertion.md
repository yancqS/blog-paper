---
title: What does the "as const" mean in TypeScript and what is its use case?
description: What does the "as const" mean in TypeScript and what is its use case?
date: '2023-02-14'
cover: 'http://img.up-4ever.site/20230214225241.jpg'
top_img: 'http://img.up-4ever.site/infinity-12769278.jpg'
tags:
  - TypeScript
categories:
  - TypeScript
abbrlink: e138f792
---

# What does the "as const" mean in TypeScript and what is its use case?

在stack overflow 上看到这样一个问题：[What does the "as const" mean in TypeScript and what is its use case?](https://stackoverflow.com/questions/66993264/what-does-the-as-const-mean-in-typescript-and-what-is-its-use-case)，其中最高票的回答让人收获匪浅，于是转载至博客。

---

This is known as a [`const` assertion](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-4.html#const-assertions). A `const` assertion tells the compiler to infer the *narrowest** or *most specific* type it can for an expression. If you leave it off, the compiler will use its default type inference behavior, which will possibly result in a *wider* or *more general* type.

Note that it is called an "assertion" and not a "cast". The term "cast" is generally to be avoided in TypeScript; when people say "cast" they often imply some sort of effect that can be observed at runtime, but TypeScript's type system, including type assertions and `const` assertions, is completely [erased](https://www.typescriptlang.org/docs/handbook/2/basic-types.html#erased-types) from the emitted JavaScript. So there is absolutely no difference *at runtime* between a program that uses `as const` and one that does not.

At compile time, though, there is a noticeable difference. Let's see what happens when you leave out `as const` in the above example:

```typescript
const args = [8, 5];
// const args: number[]
const angle = Math.atan2(...args); // error! Expected 2 arguments, but got 0 or more.
console.log(angle);
```

The compiler sees `const args = [8, 5];` and infers the type of `number[]`. That's a mutable array of zero or more elements of type `number`. The compiler has no idea *how many* or *which* elements there are. Such an inference is generally reasonable; often, array contents are meant to be modified in some way. If someone wants to write `args.push(17)` or `args[0]++`, they'll be happy with a type of `number[]`.

Unfortunately the next line, `Math.atan2(...args)`, results in an error. The `Math.atan2()` function requires exactly two numeric arguments. But all the compiler knows about `args` is that it's an array of numbers. It has completely forgotten that there are two elements, and so the compiler complains that you are calling `Math.atan2()` with "0 or more" arguments when it wants exactly two.

Compare that to the code with `as const`:

```typescript
const args = [8, 5] as const;
// const args: readonly [8, 5]
const angle = Math.atan2(...args); // okay
console.log(angle);
```

Now the compiler infers that `args` is of type `readonly [8, 5]`... a [`readonly` tuple](https://www.typescriptlang.org/docs/handbook/2/objects.html#readonly-tuple-types) whose values are exactly the numbers `8` and `5` in that order. Specifically, `args.length` is known to be exactly `2` by the compiler.

And this is enough for the next line with `Math.atan2()` to work. The compiler knows that `Math.atan2(...args)` is the same as `Math.atan2(8, 5)`, which is a valid call.

And again: at runtime, there is no difference whatsoever. Both versions log `1.0121970114513341` to the console. But `const` assertions, like the rest of the static type system, are not meant to have effects at runtime. Instead, they let the compiler know more about the *intent* of the code, and can more accurately tell the difference between correct code and bugs.

[Playground link to code](https://www.typescriptlang.org/play?ssl=12&ssc=24&pln=9&pc=1#code/HYQwtgpgzgDiDGEAEA5A9gJzCANkg3gFBIlLxrBQAuSIGA5lEgLxIDaAHADRICsAugG5ipAPSiyFarQZQAXEmABXMACMIGNvxElylGiGD0cyVgFkQVABYA6S4YBMAChuu6jAJSCk4pBoyYAIRIAKIAHjAQ8FQQACZIDjL0KhDAVFA8qko09Gg0AAxImEhgmBA2OpKUaCY2OGj0TobGEF6EAL6EhKCQsAjIAMJSVACCUFAaVACWFASVetLuTKycPAK0TAtUwqQ+EltJ8kgYECCxFDgAnuzcfNq7B80mLEgW1nZUji5usm0PUjVyvVGk9WsJOoQgA)



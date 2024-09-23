---
title: 在 TS 中如何安全的使用 `JSON.parse()`
description: 在 TS 中如何安全的使用 `JSON.parse()`
date: '2023-12-09'
cover: 'https://img.up-4ever.site/infinity-4643642.jpg'
top_img: 'http://img.up-4ever.site/infinity-4643642.jpg'
tags:
  - TypeScript
categories:
  - TypeScript
abbrlink: 4a57c4a8
---
# 在 TS 中如何安全的使用 `JSON.parse()`

我们来谈论下大家都比较熟悉的 `JSON.parse()`，大家都知道我们可以使用此方法来完成将一个 json 字符串解析为一个对象。比如
```json
const jsonString = '{ "title": "Title" }'

const jsonValue = JSON.parse(jsonString)
// { title: 'Title' } ✅
```
## 问题
你是否想过咋样一个问题：当你搞乱 json 字符串的值时会发生什么？比如将上面代码示例中的 `'{ "title": "Title" }'`修改为 `''`。那么 `JSON.parse()`会抛出一个错误：
```json
const jsonString = ''

const jsonValue = JSON.parse(jsonString)
// Error: Unexpected end of JSON input ❌ 💣
```
这看起来是一个小问题，但是大多数的开发者可能忘记正确的处理这个问题。
在 TypeScript 中，`JSON.parse()` 没有任何特定类型参数, 它只返回 `any`类型。 这意味着，它是以失去静态类型和类型安全的优势为代价的。
```typescript
interface JSON {
  /**
     * Converts a JavaScript Object Notation (JSON) string into an object.
     * @param text A valid JSON string.
     * @param reviver A function that transforms the results. This function is called for each member of the object.
     * If a member contains nested objects, the nested objects are transformed before the parent object is.
     */
  parse(text: string, reviver?: (this: any, key: string, value: any) => any): any;
  /**
     * Converts a JavaScript value to a JavaScript Object Notation (JSON) string.
     * @param value A JavaScript value, usually an object or array, to be converted.
     * @param replacer A function that transforms the results.
     * @param space Adds indentation, white space, and line break characters to the return-value JSON text to make it easier to read.
     */
  stringify(value: any, replacer?: (this: any, key: string, value: any) => any, space?: string | number): string;
  /**
     * Converts a JavaScript value to a JavaScript Object Notation (JSON) string.
     * @param value A JavaScript value, usually an object or array, to be converted.
     * @param replacer An array of strings and numbers that acts as an approved list for selecting the object properties that will be stringified.
     * @param space Adds indentation, white space, and line break characters to the return-value JSON text to make it easier to read.
     */
  stringify(value: any, replacer?: (number | string)[] | null, space?: string | number): string;
}
```
当开发人员将潜在不安全的数据视为完全安全时，可能会导致一些意想不到的问题。
## 解法

1. 写一个 `safeJsonParse`函数
```typescript
const safeJsonParse = <T>(str: string) => {
  try {
    const jsonValue: T = JSON.parse(str);
    return jsonValue;
  } catch {
    return undefined;
  }
};
// const safeJsonParse: <T>(str: string) => T | undefined ✅
```

2. 增加兜底
```typescript
interface JsonValue {
  title: string
}

const jsonString = '{ "title": "Title" }'

const jsonValue = safeJsonParse<JsonValue>(jsonString) || { "title": "" }
// const jsonValue: JsonValue ✅
```
为了防止 `jsonValue`的值为 `undefined`，我们可以添加一个兜底机制。 这确保了无论发生什么情况，`jsonValue`始终是有值的。
## 结论
因此，如果我们正确的处理这些小问题，我们就能避免一些令人头疼的问题，并使我们的代码变得更加健壮。

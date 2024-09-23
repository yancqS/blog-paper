---
title: Cookie、Session、WebStorage
date: '2018-10-10'
cover: 'http://img.up-4ever.site/20210420223609.png'
top_img: 'http://img.up-4ever.site/infinity-4164366.jpg'
tags:
  - 前端开发
  - JavaScript
categories:
  - 前端开发
abbrlink: d0ae3a51
---
# Cookie、Session、WebStorage

## Cookie

1. cookie是一段由服务器发送给浏览器并保存到本地的一段数据（缓存），大小一般为4KB.
2. cookie存储着用户的一些信息，在浏览器再次访问同一个URL时会将这段Cookie附加到HTTP请求中发送给服务端。因此，这会增加流量消耗。
3. Cookie一般在浏览器关闭（会话结束）时就被删除，但也可以通过Expire或Max-Age来设置过期时间。
4. 浏览器可以通过document.cookie读写Cookie,若要阻止此行为，可以在Set-Cookie头中添加HttpOnly标记。

## Session
Session的实质是存储在服务器里的一小块内存，一般来说是基于Cookie实现的。实现过程如下：

```js
/*
服务器端
1.当第一次使用Session时，服务器要创建一个sessionID来作为session中存放用户信息的唯一标识
*/
let session = {};
let sessionID = Math.random().toString().slice(2);/假设为 '1234567890'
session[sessionID] = userInfo;
/*2.服务器端通过Cookie给用户返回一个sessionID*/
response.setHeader('Set-Cookie',sessionID);
```

```js
/* 客户端发送的请求 */
/* 3. 当用户访问同一个 URL 时，向服务器发送这个内容为其 SessionID 的 Cookie 头 */
Request Header
HTTP/1.1 200 OK
Content-Type:text/html
Content-Length:580
Cookie:1234567890
...
```

```js
/* 服务器端 */
/* 4. 服务器读取 Cookie 中的 SessionID ，
 *    然后去 Session 中读取对应的内存，最终得到用户信息。
 */
if(request.headers.cookie)
{
let sessionID = request.headers.cookie
let userInfo = session[sessionID]
// 读取用户信息
}
```

值得一提的是， SessionID 是随机生成的。

## LocalStorage
window.localStorage 是HTML5新增的一个API，它遵循同源政策，属于本地存储。它有以下特点：

1. 大小一般为 5MB ，永久有效；
2. 本地存储和读取，不会被附加到 HTTP 请求中。

window.localStorage的使用如下例所示：

```js
// 保存数据到 LocalStorage
localStorage.setItem('key', 'value')           // {"key": "value"}
// 从 LocalStorage 获取数据
let key = localStorage.getItem('key')          // "value"
// 从 LocalStorage 删除保存的数据
localStorage.removeItem('key')                 // {}
// 从 LocalStorage 删除所有保存的数据
sessionStorage.clear();
```

## SessionStorage

window.sessionStorage同样也遵循同源政策，它与 window.localStorage不同的是，它在页面或浏览器关闭（会话结束）时就会被清除。 window.sessionStorage的使用如下例所示：

```js
// 保存数据到 SessionStorage
sessionStorage.setItem('key', 'value')
// 从 SessionStorage 获取数据
var data = sessionStorage.getItem('key')
// 从 SessionStorage 删除保存的数据
sessionStorage.removeItem('key')
// 从 SessionStorage 删除所有保存的数据
sessionStorage.clear()
```

## 总结

四者之间的比较如下：

- Cookie & Session
    
    Cookie 保存在客户端的硬盘里，是浏览器缓存之一；而 Session 是保存在服务端的内存里，是一块内存。

    Cookie 每次都随着 HTTP 请求发送给服务器；而 Session 只需要通过发送保存在 Cookie 头中的 SessionID 即可从服务器中读取对应内存。
    
    Cookie 以明文的形式存储，内容可以被用户查看或篡改；而 Session 因为只提供一个随机的 SessionID ，所以无法被用户直接查看。

- Cookie & LocalStorage

    Cookie 的大小一般为 4KB ；而 LocalStorage 一般为 5MB 。
    
    Cookie 一般在浏览器关闭（会话结束）时就失效，后端可以设置 Expires 或 Max-Age 来改变 Cookie 的过期时间；而 LocalStorage 是永久有效，除非用户手动清除。
    
    Cookie 会被附加到 HTTP 请求中；而 LocalStorage 不会。

- LocalStorage & SessionStorage
    
    前者永久有效，除非用户删除；后者在浏览器关闭（会话结束）后就被清空。

## 参考文章

- [Cookie, Session 和 WebStorage](https://zhuanlan.zhihu.com/p/42505938)
- [HTTP 牌的 Cookie](https://caijialinxx.github.io/2018/07/12/cookie/)
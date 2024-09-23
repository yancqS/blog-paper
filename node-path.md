---
title: node核心模块——path
date: '2020-04-20'
tags:
  - 前端开发
  - JavaScript
  - node
categories:
  - node
cover: 'http://img.up-4ever.site/20201017192245.jpg'
top_img: 'http://img.up-4ever.site/infinity-12769278.jpg'
abbrlink: b6932638
---
# node module: [path](http://nodejs.cn/api/path.html 'node中文官网path核心模块')

- **path.basename(path [,ext]) 方法返回 path 的最后一部分**

```javascript
path.basename('/foo/bar/baz/asdf/quux.html');
// 返回: 'quux.html'

path.basename('/foo/bar/baz/asdf/quux.html', '.html');
// 返回: 'quux'
```

---

- **path.delimiter 提供平台特定的路径定界符**

```javascript
console.log(process.env.PATH);
// 打印: 'C:\Windows\system32;C:\Windows;C:\Program Files\node\'

process.env.PATH.split(path.delimiter);
// 返回: ['C:\\Windows\\system32', 'C:\\Windows', 'C:\\Program Files\\node\\']
```

---

- **path.dirname(path) 返回 path 的目录名**

```javascript
path.dirname('D:\\YanProject\\JS\\AbortController.js')
//返回: D:\YanProject\JS
```

---

- **path.extname(path) 返回 path 的扩展名**

*ps: path.extname() 方法返回 path 的扩展名，从最后一次出现 .（句点）字符到 path 最后一部分的字符串结束。 如果在 path 的最后一部分中没有 . ，或者如果 path 的基本名称（参阅 path.basename()）除了第一个字符以外没有 .，则返回空字符串。*

```javascript
path.extname('index.html');
// 返回: '.html'

path.extname('index.coffee.md');
// 返回: '.md'

path.extname('index.');
// 返回: '.'

path.extname('index');
// 返回: ''

path.extname('.index');
// 返回: ''

path.extname('.index.md');
// 返回: '.md'
```

---

- **path.format(pathObject) 从对象返回路径字符串**

  - pathObject:
    - dir
    - root
    - base
    - name
    - ext
  
  - 如果提供了 pathObject.dir，则忽略 pathObject.root。

  - 如果 pathObject.base 存在，则忽略 pathObject.ext 和 pathObject.name。

- [POSIX](https://baike.baidu.com/item/%E5%8F%AF%E7%A7%BB%E6%A4%8D%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E6%8E%A5%E5%8F%A3/12718298?fromtitle=POSIX&fromid=3792413&fr=aladdin 'POSIX')

```javascript
// 如果提供了 `dir`、 `root` 和 `base`，
// 则返回 `${dir}${path.sep}${base}`。
// `root` 会被忽略。
path.format({
  root: '/ignored',
  dir: '/home/user/dir',
  base: 'file.txt'
});
// 返回: '/home/user/dir/file.txt'

// 如果未指定 `dir`，则使用 `root`。 
// 如果只提供 `root`，或 'dir` 等于 `root`，则将不包括平台分隔符。 
// `ext` 将被忽略。
path.format({
  root: '/',
  base: 'file.txt',
  ext: 'ignored'
});
// 返回: '/file.txt'

// 如果未指定 `base`，则使用 `name` + `ext`。
path.format({
  root: '/',
  name: 'file',
  ext: '.txt'
});
// 返回: '/file.txt'
```

- windows

```javascript
path.format({
  dir: 'C:\\path\\dir',
  base: 'file.txt'
});
// 返回: 'C:\\path\\dir\\file.txt'
```

---

- **path.isAbsolute(path) 方法检测 path 是否为绝对路径**

  - POSIX

  ```javascript
  path.isAbsolute('/foo/bar'); // true
  path.isAbsolute('/baz/..');  // true
  path.isAbsolute('qux/');     // false
  path.isAbsolute('.');        // false
  ```

  - window

  ```javascript
  path.isAbsolute('//server');    // true
  path.isAbsolute('\\\\server');  // true
  path.isAbsolute('C:/foo/..');   // true
  path.isAbsolute('C:\\foo\\..'); // true
  path.isAbsolute('bar\\baz');    // false
  path.isAbsolute('bar/baz');     // false
  path.isAbsolute('.');           // false
  ```

---

- **path.join([...paths]) 使用平台特定的分隔符作为定界符将所有给定的 path 片段连接在一起，然后规范化生成的路径**

```javascript
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..');
// 返回: '/foo/bar/baz/asdf'

path.join('foo', {}, 'bar');
// 抛出 'TypeError: Path must be a string. Received {}'
```

---

- **path.normalize(path) 规范化给定的 path，解析 '..' 和 '.' 片段**

```javascript
path.normalize('/foo/bar//baz/asdf/quux/..');
// 返回: '/foo/bar/baz/asdf'

path.normalize('C:\\temp\\\\foo\\bar\\..\\');
// 返回: 'C:\\temp\\foo\\'
```

---

- **path.parse(path) 返回一个对象，其属性表示 path 的重要元素**

  *ps: 返回的对象将具有以下属性:*

  - *dir*
  - *root*
  - *base*
  - *name*
  - *ext*

- windows

```javascript
path.parse('C:\\path\\dir\\file.txt');
// 返回:
// { root: 'C:\\',
//   dir: 'C:\\path\\dir',
//   base: 'file.txt',
//   ext: '.txt',
//   name: 'file' }
```

- POSIX

```javascript
path.parse('/home/user/dir/file.txt');
// 返回:
// { root: '/',
//   dir: '/home/user/dir',
//   base: 'file.txt',
//   ext: '.txt',
//   name: 'file' }
```

---

- **path.relative(from, to) 方法根据当前工作目录返回 from 到 to 的相对路径**
*ps:如果将零长度的字符串传入 from 或 to，则使用当前工作目录代替该零长度的字符串*

- windows

```javascript
path.relative('C:\\orandea\\test\\aaa', 'C:\\orandea\\impl\\bbb');
// 返回: '..\\..\\impl\\bbb'
```

- POSIX

```javascript
path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb');
// 返回: '../../impl/bbb'
```

---

- **path.resolve([...paths])方法将路径或路径片段的序列解析为绝对路径**

  *ps:给定的路径序列从右到左进行处理，每个后续的 path 前置，直到构造出一个绝对路径。 例如，给定的路径片段序列：/foo、 /bar、 baz，调用 path.resolve('/foo', '/bar', 'baz') 将返回 /bar/baz。如果在处理完所有给定的 path 片段之后还未生成绝对路径，则再加上当前工作目录。生成的路径已规范化，并且除非将路径解析为根目录，否则将删除尾部斜杠。零长度的 path 片段会被忽略。如果没有传入 path 片段，则 path.resolve() 将返回当前工作目录的绝对路径。*

```javascript
console.log(path.resolve('D:\\www', 'eee', 'ttt'));
//返回 D:\www\eee\ttt
console.log(path.resolve('www', 'eee', 'ttt'));
//返回 D:\node_learn\path\www\eee\ttt
console.log(path.resolve());
//返回 D:\node_learn\path
```

---

- **path.sep (这个是属性)提供平台特定的路径片段分隔符**

```javascript
console.log(path.sep);
//返回 \
```

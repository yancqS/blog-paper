---
title: node核心模块——fs
date: '2020-05-08'
tags:
  - 前端开发
  - JavaScript
  - node
categories:
  - node
cover: 'http://img.up-4ever.site/20201017192245.jpg'
top_img: 'http://img.up-4ever.site/infinity-12769278.jpg'
abbrlink: 8e03a6d3
---
# node module [fs](http://nodejs.cn/api/fs.html#fs_file_system 'fs核心模块')

>注：斜体加粗的api是有疑问的。

- 所有文件系统操作都具有同步和异步的形式

- 大多数 fs 操作接受的文件路径可以指定为字符串、Buffer、或使用 file:协议的 URL对象

- [文件描述符](https://www.jianshu.com/p/a2df1d402b4d '文件描述符'): 在 POSIX 系统上，对于每个进程，内核都维护着一张当前打开着的文件和资源的表格。 每个打开的文件都分配了一个称为文件描述符的简单的数字标识符。 在系统层，所有文件系统操作都使用这些文件描述符来标识和跟踪每个特定的文件。 Windows 系统使用了一个虽然不同但概念上类似的机制来跟踪资源。 为了简化用户的工作，Node.js 抽象出操作系统之间的特定差异，并为所有打开的文件分配一个数字型的文件描述符。
  
---

- **fs.Dir代表目录流的类**

  - *由 fs.opendir()、fs.opendirSync() 或 fsPromises.opendir() 创建。*

  - dir.close() 返回promise，将会在关闭资源之后被resolve。这个方法的作用时异步地关闭目录底层资源句柄。随后的读取将会导致错误。

  - dir.close(callback) 这个方法的作用同上，关闭资源句柄后将会调用callback。
    - callback(err)

  ```javascript
  //个人理解
  const dir = fs.opendir(path);

  dir.close((err) => {
    //doSomething
  })
  ```

  - dir.closeSync() 同步地关闭底层资源句柄。

  - dir.path <属性>，此目录的只读路径，与提供给 fs.opendir()、fs.opendirSync() 或 fsPromises.opendir() 的一样。

  - dir.read() 返回: Promise 包含 fs.Dirent或null。异步地读取下一个目录项作为 fs.Dirent。读取完成之后，将会返回一个 Promise，它被解决时将会返回 fs.Dirent 或 null（如果没有更多的目录项要读取）。此函数返回的目录项不遵循操作系统的底层目录机制所提供的特定顺序。 遍历目录时添加或删除的目录项可能会也可能不会包含在遍历的结果中。

  - dir.read(callback) 异步地读取下一个目录项作为 fs.Dirent。读取完成之后，将会调用 callback 并传入 fs.Dirent 或 null（如果没有更多的目录项要读取）。callback((err, fs.dirent) => {});

  - dir.readSync() 返回: Promise 包含 fs.Dirent或null。同步地读取下一个目录项作为 fs.Dirent。

---

- **fs.Dirent类 目录项的表示形式，通过从 fs.Dir 中读取得到**

  - dirent.isBlockDevice()
    - 返回布尔值 (如果 fs.Dirent 对象描述块设备，则返回true)
  
  - dirent.isCharacterDevice()
    - 返回布尔值 (如果 fs.Dirent 对象描述字符设备，则返回true)

  - dirent.isDirectory()
    - 返回布尔值 (如果 fs.Dirent 对象描述文件系统目录，则返回true)
  
  - dirent.isFIFO()
    - 返回布尔值(如果 fs.Dirent 对象描述先进先出（FIFO）管道，则返回true)
  
  - dirent.isFile()
    - 返回布尔值(如果 fs.Dirent 对象描述常规文件，则返回 true)

  - dirent.isSymbolicLink()
    - 返回布尔值(如果 fs.Dirent 对象描述符号链接，则返回 true)

  - dirent.name
    - 属性，fs.Dirent 对象指向的文件名。 此值的类型取决于传递给 fs.readdir() 或 fs.readdirSync() 的 options.encoding

  ```javascript
  //假设当前目录下有以下结构
  // - file(文件夹)
  // - fs.js
  // - fs.md
  async function print(path) {
  const dir = await fs.promises.opendir(path);
  for await (const dirent of dir) {
      console.log(dirent);
      // Dirent { name: 'file', [Symbol(type)]: 2 }
      // Dirent { name: 'file_test.lnk', [Symbol(type)]: 1 }
      // Dirent { name: 'fs.js', [Symbol(type)]: 1 }
      // Dirent { name: 'fs.md', [Symbol(type)]: 1 }

      console.log(dirent.isDirectory());
      //true false false

      console.log(dirent.isFile());
      // false true true

      console.log(dirent.name);
      //file fs.js fs.md
    }
  }
  print('./').catch(console.error);
  ```

---

- **fs.Stats类**

  >*fs.Stats对象提供了关于文件的信息。从 fs.stat()、fs.lstat()、fs.fstat()、以及它们的同步方法返回的对象都是此类型。*

  - stats.isBlockDevice()
    - 如果 fs.Stats 对象描述块设备，则返回 true。

  - stats.isCharacterDevice()
    - 如果 fs.Stats 对象描述字符设备，则返回 true。

  - stats.isDirectory()
    - 如果 fs.Stats 对象描述文件系统目录，则返回 true。

  - stats.isFIFO()
    - 如果 fs.Stats 对象描述先进先出（FIFO）管道，则返回 true。

  - stats.isFile()
    - 如果 fs.Stats 对象描述普通的文件，则返回 true。

  - stats.isSocket()
    - 如果 fs.Stats 对象描述套接字，则返回 true。

  - stats.isSymbolicKink()
    - 如果 fs.Stats 对象描述符号链接，则返回 true。

  ```javascript
  //windows平台下
  fs.rename('./file/hello.txt', './file/hello_bak.txt', (err) => {
    if(err) throw err;
    fs.stat('./file/hello_bak.txt', (err, stats) => {
      if(err) throw err;
      console.log(`文件属性${JSON.stringify(stats)}`);
      console.log(stats.isFile()); //true
    })
  })
  ```

  ```javascript
  //上面代码中的console.log(`文件属性${JSON.stringify(stats)}`);
  {
        "dev":3935059853, //包含该文件的设备数字标识符

        "mode":33206, //描述文件类型和模式的位字段

        "nlink":1, //文件存在的硬链接数

        "uid":0, //拥有该文件（POSIX）的用户的数字型群组标识符

        "gid":0, //拥有该文件（POSIX）的群组的数字型群组标识符

        "rdev":0, //如果文件被视为特殊文件，则此值为数字型设备标识符

        "ino":562949953422602, //文件系统特定的文件索引节点编号

        "size":5, //文件的大小（以字节为单位）

        "atimeMs":1588206862385.9998, //表明上次访问(access)此文件的时间戳，以POSIX 纪元以来的毫秒数表示

        "mtimeMs":1588836481684.9998, //表明上次修改(modify)此文件的时戳，以POSIX 纪元以来的毫秒数表示

        "ctimeMs":1588837275795.1448, //表明上次更改(change)文件状态的时间戳，以POSIX 纪元以来的毫秒数表示

        "birthtimeMs":1588206862386.4824, //表明此文件的创建时间的时间戳，以POSIX 纪元以来的毫秒数表示。

        "atime":"2020-04-30T00:34:22.386Z",

        "mtime":"2020-05-07T07:28:01.685Z",

        "ctime":"2020-05-07T07:41:15.795Z",

        "birthtime":"2020-04-30T00:34:22.386Z"
      }
  ```


  |名称 | 全名 | 中文名称 | 含义|
  |:-:  | :-: | :-:  | :-:|
  |atime| access time | 访问时间 | 文件中的数据被最后访问的时间|
  |mtime| modify time | 修改时间 | 文件内容被最后修改的时间|
  |ctime| change time | 变化时间 | 文件的元数据发生变化的时间。如权限、所有者等|

---

- **fs.watch(filename[, options][, listener])**
  - filename `<string>` | `<Buffer>` | `<URL>`
  - options `<object>` | `<string>`
    - persistent `<boolean>` 指示如果文件已正被监视，进程是否应继续运行。默认值: true
    - recursive `<boolean>` 指示应该监视所有子目录，还是仅监视当前目录。这适用于监视目录时，并且仅适用于受支持的平台。默认值：false
    - encoding `<string>` 指定用于传给监听器的文件名的字符编码。默认值: 'utf8'
  - listener `<Function>` | `<undefined>`
    - eventType `<string>`
    - filename `<string>` | `<Buffer>`
  
  >*返回fs.FSWatch类。监视 filename 的更改，其中 filename 是 **文件或目录**。第二个参数是可选的。如果 options 传入字符串，则它指定 encoding。否则，options 应传入对象。监听器回调有两个参数 (eventType, filename)。eventType是'rename'或'change'，filename是触发事件的文件的名称。*

  >*监听器回调绑定在由 fs.FSWatcher 触发的 'change' 事件上，但它与 eventType 的 'change' 值不是一回事。*

  ```javascript
  fs.watch('./file', {recursive: true, encoding: 'utf8'}, (eventType, filename) => {
    console.log(eventType, filename);
  })
  ```

  ![](http://img.up-4ever.site/20201017232123.png)

---

- **fs.watchFile(filename[, options], listener)**
  - filename `<string>` | `<Buffer>` | `<URL>`
  - options `<Object>`
    - bigint `<boolean>` 默认值: false
    - persistent `<boolean>` 默认值: true
    - interval `<integer>` 默认值: 5007
  - listener
    - current /<fs.Stats>`
    - previous /<fs.Stats>`

  >*返回fs.StatWatcher。监视filename的更改。每当访问文件时都会调用 listener 回调。options 参数可以省略。如果提供，则它应该是一个对象。 options 对象可以包含一个名为 persistent 的布尔值，指示当文件正在被监视时，进程是否应该继续运行。 options 对象可以指定 interval 属性，指示轮询目标的频率（以毫秒为单位）。如果 bigint 选项为 true，则fs.Stats中的数值会被指定为 BigInts 类型。*

  >**使用 fs.watch() 比 fs.watchFile 和 fs.unwatchFile 更高效。**

  ```javascript
  fs.watchFile('./file/hello.txt', { persistent: true, interval: 5007 }, (cur, prev) => {
    console.log(new Date(cur.mtime).toLocaleString());
    console.log(new Date(cur.ctime).toLocaleString());
    console.log(new Date(prev.mtime).toLocaleString());
    console.log(new Date(prev.ctime).toLocaleString());
  })
  ```

  ![](http://img.up-4ever.site/20201017232142.jpg)

---

- **fs.unwatchFile(filename[, listener])**
  - filename `<string>` | `<Buffer>` | `<URL>`
  - listener `<Function>`

  >*停止对filename的监视。如果指定了listener，则移除此特定监视器，否则移除所有监视器，从而停止监视filename。对未被监视的文件名调用 fs.unwatchFile() 将是空操作，而不是错误。*

---

- **fs.FSWatcher类**

  > *成功调用 fs.watch() 方法将会返回一个新的 fs.FSWatcher 对象。每当指定监视的文件被修改时，所有的 fs.FSWatcher 对象都会触发 'change' 事件。*

  - 'change'事件
    - eventType `<string>` 已发生的更改事件的类型。
    - filename `<string>` | `<Buffer>` 更改的文件名（如果相关或可用）。
  
  ```javascript
  // eventType是'rename'或'change', filename 是触发事件的文件的名称。
  fs.watch('./fs.md', (eventType, filename) => {
    if(filename) {
      console.log(eventType, filename);
    }
  })
  ```

  ![](http://img.up-4ever.site/20201017232203.png)

  - 'close' 事件
    - 当监视器停止监视更改时触发

  ```javascript
  let fsWatch = fs.watch('./fs.md', (eventType, filename) => {
    if(filename) {
      console.log(eventType, filename);
    }
  });
  setTimeout(() => { fsWatch.close() }, 5000);
  fsWatch.on('close', () => {
    console.log('close');
  })
  ```

  ![](http://img.up-4ever.site/20201017232230.jpg)

  - 'error' 事件
    - 当监视文件时发生错误时触发。

  - watcher.close()
    - 给定的 fs.FSWatcher 停止监视更改。

    ```javascript
    //上面一段代码中的
    setTimeout(() => { fsWatch.close() }, 5000);
    ```

---

- **fs.createReadStream(path[, options])**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<string>` | `<Object>`
    - flags `<string>` 参见文件系统 flag 的支持。 默认值: 'r'
    - encoding `<string>` 默认值: null
    - fd `<integer>` 默认值: null
    - mode `<integer>` 默认值: 0o666
    - autoClose `<boolean>` 默认值: true
    - emitClose `<boolean>` 默认值: false
    - start `<integer>`
    - end `<integer>` 默认值: Infinity
    - highWaterMark `<integer>` 默认值: 64 * 1024
    - fs  `<Object>` | `<null>` 默认值: null
  
  >*返回fs.ReadStream 类。如果options指定了 fd，则 ReadStream 会忽略 path 参数，并且会使用指定的文件描述符。*

  >*highWaterMark内部缓冲区最多能容纳的字节数，如果超过这个大小，就停止读取资源文件，默认值是64KB。比如有一个100KB的文件，设置highWaterMark为10KB,那么系统会先从资源文件中读取出10KB的数据，再触发data事件;然后再读取10KB的数据，触发data事件，不断执行，直到读出了所有的数据，触发end事件。highWaterMark不能设置的过小，过小就会频繁的操作文件，影响性能。*

---

- **fs.ReadStream 类**
  - [How to use fs.createReadStream?](https://nodejs.org/en/knowledge/advanced/streams/how-to-use-fs-create-read-stream/ 'How to use fs.createReadStream?')

  - [Node.js文件系统模块fs创建可读流与可写流](https://itbilu.com/nodejs/core/Eyg7MCMMe.html 'Node.js文件系统模块fs创建可读流与可写流')
  
  >*成功调用fs.createReadStream()会返回新建的 fs.ReadStream 对象。*
  >*fs模块提供了创建文件可读流与创建文件可写读的方法：fs.createReadStream()和fs.createWriteStream()，这两个方法分别分返回一个Readable Stream对象和Writable Stream对象。通过这两个对象，我们可以基于流的文件处理，在读写大文件及基于流的转接、暂停、读取等操作方面有很大的作用。*

  - close事件
    - 当fs.ReadStream的底层文件描述符已关闭时触发。

  - open事件
    - 当fs.ReadStream的文件描述符打开时触发。

  - ready事件
    - 当fs.ReadStream准备好使用时触发。'open'事件之后立即触发。

  - end事件
  
  - error事件
  
  - pause事件
  
  - resume事件
  
  - data事件

  - readStream.bytesRead
    - 到目前为止已读取的字节数。

  - readStream.path
    - 流正在读取的文件的路径，由 fs.createReadStream() 的第一个参数指定。如果 path 传入字符串，则 readStream.path 将是字符串。如果 path 传入 Buffer，则 readStream.path将是Buffer。

  - readStream.pending
    - 如果底层的文件还未被打开（即在触发 'ready' 事件之前），则此属性为 true。

  ```javascript
  //hello.txt的内容为 hello
  let readStream = fs.createReadStream('./file/hello.txt');

  console.log(readStream.pending);// true

  readStream.on('open', () => {
    console.log(readStream.bytesRead);// 0
    console.log(readStream.path);// './file/hello.txt'
    console.log(readStream.pending);// false
  })

  readStream.on('data', (data) => {
    console.log(readStream.bytesRead);// 5
    console.log(data.toString()); // hello
  })

  readStream.on('error', (err) => {
    console.log(err);
  })

  readStream.on('end', () => {
    console.log('end');
  })
  ```

---

- **fs.createWriteStream(path[, options])**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<string>` | `<Object>`
    - flags `<string>` 参见文件系统 flag 的支持。 默认值: 'w'
    - encoding `<string>` 默认值: null
    - fd `<integer>` 默认值: null
    - mode `<integer>` 默认值: 0o666
    - autoClose `<boolean>` 默认值: true
    - emitClose `<boolean>` 默认值: false
    - start `<integer>`
    - end `<integer>` 默认值: Infinity
    - highWaterMark `<integer>` 默认值: 16 * 1024。如果超过这个值，write方法就会返回false
    - fs  `<Object>` | `<null>` 默认值: null
  
  >*返回fs.WriteStream 类。与 ReadStream 一样，如果指定了 fd，则 WriteStream 会忽略 path 参数，并且会使用指定的文件描述符。 这意味着不会触发 'open' 事件。*
  
---

- **fs.WriteStream类**

  >*成功调用fs.createWriteStream()会返回新建的 fs.WriteStream对象。*

  - clsoe事件
    - 当 WriteStream 的底层文件描述符已关闭时触发

  - open事件
    - 当 WriteStream 的文件打开时触发
  
  - ready事件
    - 'open' 事件之后立即触发

  - finish事件
  
  - error事件
  
  - drain事件
    - 在缓冲区数据写入完毕后触发
  
  - writeStream.bytesWritten
    - 到目前为止写入的字节数。 不包括仍在排队等待写入的数据
  
  - writeStream.path
    - 流正在写入的文件的路径，由 fs.createWriteStream() 的第一个参数指定。 如果 path 传入字符串，则 writeStream.path 将是字符串。 如果 path 传入 Buffer，则 writeStream.path 将是 Buffer。
  
  - writeStream.pending
    - 如果底层的文件还未被打开（即在触发 'ready' 事件之前），则此属性为 true。

  ```javascript
  let writeStream = fs.createWriteStream('./file/test.txt');

  console.log(writeStream.path); // ./file/test.txt

  writeStream.write('wswswsws' + new Date(), 'UTF-8');
  //写入内容格式为：wswswswsThu May 07 2020 18:22:06 GMT+0800 (GMT+08:00)

  console.log(writeStream.bytesWritten); // 0

  writeStream.end(); //标记文件末尾, 结束写入流, 释放资源

  writeStream.on( 'finish',  function() {
    console.log(writeStream.bytesWritten) // 53
    console.log("写入完成");
  });

  writeStream.on( 'error',  function(error){
    console.log(error);
  });
  ```

  ```javascript
  //一个文件拷贝的例子
  let rs = fs.createReadStream("/img/in-post/post-node-fs/timg.jpg");//默认64KB
  let ws = fs.createWriteStream("/img/in-post/post-node-fs/Copy.jpg");//默认16KB，写入速度小于读取速度
  rs.on("data", function (data) {
    let flag = ws.write(data);
    if (!flag) { //缓冲区已满
      rs.pause();//停止触发data事件
    }
  });
  ws.on("drain", function () {
    rs.resume();//如果当前的缓冲区写入完毕，就重新触发data事件
  });
  rs.on("end", function () {
    ws.end();//将剩下的数据全部写入，并且关闭写入的文件
  })
  ```

---

- **fs.access(path[, mode], callback)**
  - path 路径
  - mode **默认值:** fs.constants.F_OK (为0)
  - callback 回调函数
    - err

>*测试用户对 path 指定的文件或目录的权限。 mode 参数是一个可选的整数，指定要执行的可访问性检查。最后一个参数 callback 是回调函数，调用时会传入可能的错误参数。*

- 文件可访问性的常量(以下常量适用于 fs.access())

  常量 | 说明
  :-:  |  :-:
  F_OK | 表明文件对调用进程可见。 这对于判断文件是否存在很有用，但对 rwx 权限没有任何说明。 如果未指定模式，则默认值为该值。
  R_OK | 表明调用进程可以读取文件
  W_OK | 表明调用进程可以写入文件
  X_OK | 表明调用进程可以执行文件。 在 Windows 上无效(表现得像 fs.constants.F_OK)

  ```javascript
  const file = './file/test.txt';

  // 检查文件是否存在于当前目录中。
  fs.access(file, fs.constants.F_OK, (err) => {
    console.log(`${file} ${err ? '不存在' : '存在'}`);
  });

  // 检查文件是否可读。
  fs.access(file, fs.constants.R_OK, (err) => {
    console.log(`${file} ${err ? '不可读' : '可读'}`);
  });

  // 检查文件是否可写。
  fs.access(file, fs.constants.W_OK, (err) => {
    console.log(`${file} ${err ? '不可写' : '可写'}`);
  });

  // 检查文件是否存在于当前目录中、以及是否可写。
  fs.access(file, fs.constants.F_OK | fs.constants.W_OK, (err) => {
    if (err) {
      console.error(
        `${file} ${err.code === 'ENOENT' ? '不存在' : '只可读'}`);
    } else {
      console.log(`${file} 存在，且可写`);
    }
  });
  ```

  ```javascript
  // fs.constants
  {
    UV_FS_SYMLINK_DIR: 1,
    UV_FS_SYMLINK_JUNCTION: 2,
    O_RDONLY: 0,
    O_WRONLY: 1,
    O_RDWR: 2,
    UV_DIRENT_UNKNOWN: 0,
    UV_DIRENT_FILE: 1,
    UV_DIRENT_DIR: 2,
    UV_DIRENT_LINK: 3,
    UV_DIRENT_FIFO: 4,
    UV_DIRENT_SOCKET: 5,
    UV_DIRENT_CHAR: 6,
    UV_DIRENT_BLOCK: 7,
    S_IFMT: 61440,
    S_IFREG: 32768,
    S_IFDIR: 16384,
    S_IFCHR: 8192,
    S_IFLNK: 40960,
    O_CREAT: 256,
    O_EXCL: 1024,
    UV_FS_O_FILEMAP: 536870912,
    O_TRUNC: 512,
    O_APPEND: 8,
    F_OK: 0,
    R_OK: 4,
    W_OK: 2,
    X_OK: 1,
    UV_FS_COPYFILE_EXCL: 1,
    COPYFILE_EXCL: 1,
    UV_FS_COPYFILE_FICLONE: 2,
    COPYFILE_FICLONE: 2,
    UV_FS_COPYFILE_FICLONE_FORCE: 4,
    COPYFILE_FICLONE_FORCE: 4
  }
  ```

---

- **fs.accessSync(path[, mode])**
  - path
  - mode **默认值:** fs.constants.F_OK。

>*同步地测试用户对 path 指定的文件或目录的权限。 mode 参数是一个可选的整数，指定要执行的可访问性检查。*

  ```javascript
  try {
    fs.accessSync(file, fs.constants.R_OK | fs.constants.W_OK);
    console.log(`${file}可读写`);
  } catch (err) {
    console.log(`${file}无权限访问`);
  }
  ```

---

- **fs.appendFile(path, data[, options], callback)**
  - path `<string>` | `<Buffer>` | `<URL>` | `<number>` 文件名或文件描述符。
  - data `<string>` | `<Buffer>`
  - options `<object>` | `<string>`
    - encoding `<string>` | `<null>` 默认值: 'utf8'
    - mode `<integer>` 默认值: 0o666
    - flag `<string>` 参见文件系统 flag 的支持。默认值: 'a'。
  - callback `<Function>`
    - err `<Error>`

>异步地追加数据到文件，如果文件尚不存在则创建文件。 data 可以是字符串或 Buffer。

  ```javascript
  fs.appendFile('./file/test.txt', '\nappend data==', 'utf8', (err) => {
    if(err) throw err;
      console.log('data has been appended');
  })

  //path可以指定为已打开用于追加(使用fs.open()或fs.openSync())的数字型文件描述符。文件描述符不会自动关闭
  fs.open('./file/test.txt', 'a', (err, fd) => {
    if(err) throw err;
    fs.appendFile(fd, 'append data by fd', 'utf8', (err) => {
      fs.close(fd, (err) => {
        if(err) throw err;
      });
      if(err) throw err;
    })
  })
  ```

  ![](http://img.up-4ever.site/20201017232253.png)

  ![](http://img.up-4ever.site/20201017232304.png)

---

- **fs.appendFileSync(path, data[, options])**
  - path `<string>` | `<Buffer>` | `<URL>` | `<number>` 文件名或文件描述符。
  - data `<string>` | `<Buffer>`
  - options `<object>` | `<string>`
    - encoding `<string>` | `<null>` 默认值: 'utf8'
    - mode `<integer>` 默认值: 0o666
    - flag `<string>` 参见文件系统 flag 的支持。默认值: 'a'。

>*同步地将数据追加到文件，如果文件尚不存在则创建该文件。 data 可以是字符串或 Buffer*

  ```javascript
  //如果options是字符串，则它指定字符编码
  try {
    fs.appendFileSync('./file/test.txt', '\nappend data sync', 'utf8');
    console.log('data has append file Sync')
  } catch (err) {
    console.log(err);
  }

  //path可以指定为已打开用于追加(使用fs.open()或fs.openSync())的数字型文件描述符。文件描述符不会自动关闭

  let fd;

  try {
    fd = fs.openSync('文件.txt', 'a');
    fs.appendFileSync(fd, '追加的数据', 'utf8');
  } catch (err) {
    /* 处理错误 */
  } finally {
    if (fd !== undefined)
      fs.closeSync(fd);
  }
  ```

---

- **fs.chmod(path, mode, callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - mode `<string>` | `<integer>`
  - callback `<Function>`
    - err
  
  >*异步地更改文件的权限*
  
  ```javascript
  //r、w、x ==> 4、2、1
  //user(rwx) group(r-x) other(r--)
  //在windows下权限修改没有成功，但是不会报错(在gitBash环境下验证的)
  fs.chmod('./file/test.txt', 0o666, (err) => {
    if(err) throw err;
    console.log('文件test.txt权限已更改');
  })
  ```

---

- **fs.chmodSync(path, mode)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - mode `<string>` | `<integer>`
  
  >*同步地更改文件的权限*

---

- **fs.fchmod(fd, mode, callback)**
  - fd `<integer>`
  - mode `<string>` | `<integer>`
  - callback `<Function>`
    - err

  >*这个也是异步地修改文件的权限，但是它和fs.chmod(path, mode, callback)不同的是，fs.fchmod(fd, mode, callback)传入的第一个参数是文件描述符，而fs.chmod(path, mode, callback)传入的第一个参数是文件路径*

---

- **fs.fchmodSync(fd, mode)**
  - fd `<integer>`
  - mode `<string>` | `<integer>`

  >*同步地修改文件的权限(传入文件描述符)*

---

- **fs.lchmode(path, mode, callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - mode `<integer>`
  - callback `<Function>`
    - err `<Error>`
  
  >*异步地更改文件权限（不解析符号链接），即如果 path 是符号链接，则查看的是链接自身，而不是它指向的文件。*

  >*The lchmod() system call is similar to chmod() but does not follow symbolic links.(???)仅适用于 macOS。*

---

- **fs.chown(path, uid, gid, callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - uid `<integer>`
  - gid `<integer>`
  - callback `<Function>`
    - err
  
  >*异步地更改文件的所有者(user)和群组(group)*

---

- **fs.chownSync(path, uid, gid)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - uid `<integer>`
  - gid `<integer>`
  
  >*同步地更改文件的所有者和群组。返回 undefined。这是 fs.chown()的同步版本*

---

- **fs.fchown(fd, uid, gid, callback)**
  - fd `<integer>`
  - uid `<integer>`
  - gid `<integer>`
  - callback `<Function>`
    - err

  >*异步地更改文件(传入文件描述符)的所有者(user)和群组(group)*

---

- **fs.fchownSync(fd, uid, gid)**
  - fd `<integer>`
  - uid `<integer>`
  - gid `<integer>`

  >*同步地更改文件(传入文件按描述符)的所有者和群组。返回 undefined。这是 fs.fchown()的同步版本*

---

- **fs.lchown(path, uid, gid, callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - uid `<integer>`
  - gid `<integer>`
  - callback `<Function>`
    - err

  >*异步地更改文件的所有者(user)和群组(group)，不解析符号链接，即如果 path 是符号链接，则查看的是链接自身，而不是它指向的文件。*

  >*lchown() is like chown(), but does not dereference symbolic links.*

---

- **fs.lchownSync(path, uid, gid)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - uid `<integer>`
  - gid `<integer>`

  >*同步地更改文件(传入文件按描述符)的所有者和群组，不解析符号链接，即如果 path 是符号链接，则查看的是链接自身，而不是它指向的文件。返回 undefined*

---

- **fs.close(fd, callback)**
  - fd `<integer>`
  - callback `<Function>`
    - err

  >*异步地关闭文件描述符*
  
---

- **fs.closeSync(fd)**
  - fd `<integer>`

  >*同步地关闭文件描述符*
  
---

- **fs.copyFile(src, dest[, mode], callback)**
  - src `<string>` | `<Buffer>` | `<URL>` 要拷贝的源文件名
  - dest `<string>` | `<Buffer>` | `<URL>` 拷贝操作的目标文件名
  - mode `<integer>` 用于拷贝操作的修饰符。默认值: 0
  - callback `<Function>`
    - err
  
  >*异步地将 src 拷贝到 dest。 默认情况下，如果 dest 已经存在，则覆盖它。 除了可能的异常，回调函数没有其他参数。mode 是一个可选的整数，指定拷贝操作的行为。可以创建由两个或更多个值按位或组成的掩码（比如 fs.constants.COPYFILE_EXCL \| fs.constants.COPYFILE_FICLONE）*

  >- fs.constants.COPYFILE_EXCL - 如果 dest 已存在，则拷贝操作将失败
  >- fs.constants.COPYFILE_FICLONE - 拷贝操作将尝试创建写时拷贝（copy-on-write）链接。如果平台不支持写时拷贝，则使用后备的拷贝机制
  >- fs.constants.COPYFILE_FICLONE_FORCE - 拷贝操作将尝试创建写时拷贝链接。如果平台不支持写时拷贝，则拷贝操作将失败

  ```javascript
  function copyFile(mkdir = '') {
    // 通过使用 COPYFILE_EXCL，如果目标文件存在，则操作将失败。
    // fs.copyFile('源文件.txt', '目标文件.txt', fs.constants.COPYFILE_EXCL, callback);
    fs.copyFile('./file/test.txt', './file_bak/test.txt', (err) => {
      if(err) throw err;
      console.log('copy successfully', mkdir == 'mkdir' ? 'mkdir' : '');
    });
  }

  //先判断文件夹是否存在
  if(!fs.existsSync('./file_bak')) {
    fs.mkdir('./file_bak', (err) => {
      if(err) throw err;
      copyFile('mkdir');
    })
  } else {
    copyFile();
  }
  ```

---

- **fs.copyFileSync(src, dest[, mode])**
  - src `<string>` | `<Buffer>` | `<URL>` 要拷贝的源文件名
  - dest `<string>` | `<Buffer>` | `<URL>` 拷贝操作的目标文件名
  - mode `<integer>` 用于拷贝操作的修饰符。默认值: 0

  > *同步地将src拷贝到dest。默认情况下，如果dest已经存在，则覆盖它。返回undefined。如果在打开目标文件用于写入后发生错误，则 Node.js 将尝试删除目标文件。mode 是一个可选的整数，指定拷贝操作的行为。 可以创建由两个或更多个值按位或组成的掩码（比如 fs.constants.COPYFILE_EXCL \| fs.constants.COPYFILE_FICLONE）。*

---

- **fs.existsSync(path)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - 返回布尔值
  
  ```javascript
  //判断路径(文件夹/文件)是否存在
  if(!fs.existsSync('./file_bak')) {
    //doSomething
  } else {
    //doSomething
  }
  ```

---

- **fs.fdatasync(fd, callback)**
  - fd `<integer>`
  - callback `<Function>`
    - err

  >*异步地刷新数据到磁盘*

  ```javascript
  // Open the file
  fs.open(filename, "a+", (err, fd) => {
    // Write our data
    fs.writeFile(fd, data, (err) => {
        // Force the file to be flushed
        fs.fdatasync(fd /*, optional callback here */);
    });
  });
  ```

---

- **fs.fdatasyncSync(fd)**
  - fd `<integer>`
  
  >*fs.fdatasync(fd, callback)的同步版本。同步地刷新数据到磁盘。*

---

- **fs.stat(path[, options], callback)**
  - path  `<string>` | `<Buffer>` | `<URL>`
  - options `<Object>`
    - bigint `<boolean>`返回的fs.Stats对象中的数值是否为bigint型。默认值: false。
  - callback `<Function>`
    - err `<Error>`
    - stats `<fs.Stats>`
  
  >*异步地查看文件的属性信息。如果文件不存在，回调函数中stats为undefined,若要只检查文件是否存在，但没有更多的操作，则建议使用 fs.access()*

  ```javascript
  fs.stat('./file/test.txt', (err, stats) => {
    console.log(stats ? '文件存在' : '文件不存在');
    console.log(JSON.stringify(stats));
    //文件存在：{"dev":3935059853,"mode":33206,...}
    //文件不存在: undefined
  })


  const file = './file/test.txt';
  // 检查文件是否存在于当前目录中。
  fs.access(file, fs.constants.F_OK, (err) => {
    console.log(`${file} ${err ? '不存在' : '存在'}`);
  });
  ```
  
---

- **fs.statSync(path[, options])**
  - path  `<string>` | `<Buffer>` | `<URL>`
  - options `<Object>`
    - bigint `<boolean>`返回的fs.Stats对象中的数值是否为bigint型。默认值: false。
  - 返回`<fs.Stats>`

  >*同步地查看文件的属性信息。*

  ```javascript
  try {
    let stat = fs.statSync('./file/test.txt');
    console.log(stat);
  } catch (err) {
    console.log(err.code);
    throw err;
  }
  ```

---

- **fs.fstat(fd[, options], callback)**
  - fd `<integer>`
  - options `<Object>`
    - bigint `<boolean>`返回的fs.Stats对象中的数值是否为bigint型。默认值: false。
  - callback `<Function>`
    - err `<Error>`
    - stats `<fs.Stats>`
  
  >*异步地查看文件的属性信息。与fs.stat(path[, options], callback)不同的是，fs.fstat(fd[, options], callback)传入文件描述符。*

  ```javascript
  fs.open('./file/test.txt', 'r', (err, fd) => {
    if(err && err.code == 'ENOENT') {
      console.log('文件不存在');
      return;
    };
    fs.fstat(fd, (err, stats) => {
      if(err) throw err;
      console.log(stats);
    })
  })
  ```

---

- **fs.fstatSync(fd[, options])**
  - fd `<integer>`
  - options `<Object>`
    - bigint `<boolean>`返回的fs.Stats对象中的数值是否为bigint型。默认值: false。
  
  >*同步地查看文件的属性信息，传入文件描述符*

---

- **fs.lstat(path[, options], callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<Object>`
    - bigint `<boolean>`
  - callback `<Function>`
    - err
    - stats

  >*回调会传入两个参数 (err, stats)，其中stats是fs.Stats对象。lstat()与stat()相同，**只是如果path是符号链接，则查看的是链接自身，而不是它指向的文件。**(这个难道就是传说中的不解析符号链接吗？？)*

---

- **fs.lstatSync(path[, options])**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<Object>`
    - bigint `<boolean>`

  >*返回fs.Stats*

---

- **fs.fsync(fd, callback)**
  - fd `<integer>`
  - callback `<Function>`
    - err `<Error>`
  
  >*异步地刷新数据到磁盘*

  >*fsync() transfers ("flushes") all modified in-core data of (i.e., modified buffer cache pages for) the file referred to by the file descriptor fd to the disk device (or other permanent storage device)*

  [fs.fsync(fd, callback)和fs.fdatasync(fd, callback)的区别](https://linux.die.net/man/2/fsync 'fs.fsync(fd, callback)和fs.fdatasync(fd, callback)的区别')

  >*fdatasync() is similar to fsync(), but does not flush modified metadata unless that metadata is needed in order to allow a subsequent data retrieval to be correctly handled. For example, changes to st_atime or st_mtime (respectively, time of last access and time of last modification; see stat(2)) do not require flushing because they are not necessary for a subsequent data read to be handled correctly. On the other hand, a change to the file size (st_size, as made by say ftruncate(2)), would require a metadata flush.*

---

- **fs.fsyncSync(fd)**
  - fd `<integer>`

  > *同步地刷新数据到磁盘。返回undefined*

---

- **fs.ftruncate(fd[, len], callback)**
  - fd `<integer>`
  - len `<integer>` **默认值:** 0
  - callback `<Function>`
    - err `<Error>`

  >*如果文件描述符指向的文件大于 len 个字节，则只有前面 len 个字节会保留在文件中。如果文件小于 len 个字节，则会对其进行扩展，并且扩展部分将填充空字节（'\0'）*

  ```javascript
  console.log(fs.readFileSync('./file/hello.txt', 'utf8'));
  const fd = fs.openSync('./file/hello.txt', 'r+');
  fs.ftruncate(fd, 4, (err) => {
    if(err) throw err;
    console.log(fs.readFileSync('./file/hello.txt', 'utf8'));
  })
  ```

  ![](http://img.up-4ever.site/20201017232336.jpg)

  ![](http://img.up-4ever.site/20201017232349.jpg)

---

- **fs.ftruncateSync(fd[, len])**
  - fd `<integer>`
  - len `<integer>` **默认值:** 0

  >*返回undefined。fs.ftruncate(fd[, len], callback)的同步版本*

---

- **fs.truncate(path[, len], callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - len `<integer>` **默认值:** 0
  - callback `<Function>`
    - err `<Error>`

  >*如果文件描述符指向的文件大于 len 个字节，则只有前面 len 个字节会保留在文件中。如果文件小于 len 个字节，则会对其进行扩展，并且扩展部分将填充空字节（'\0'）*

---

- **fs.truncateSync(path[, len])**
  - path `<string>` | `<Buffer>` | `<URL>`
  - len `<integer>` **默认值:** 0

  >*返回undefined。fs.truncate(path[, len], callback)的同步版本*

---

- **fs.futimes(fd, atime, mtime, callback)**
  - fd `<integer>`
  - atime `<number>` | `<string>` | `<Date>` 访问时间
  - mtime `<number>` | `<string>` | `<Date>` 修改时间
  - callback `<Function>`
    - err `<Error>`

  >*更改文件描述符指向对象的文件系统时间戳*

---

- **fs.futimesSync(fd, atime, mtime)**
  - fd `<integer>`
  - atime `<number>` | `<string>` | `<Date>`
  - mtime `<number>` | `<string>` | `<Date>`
  
  >*fs.futimes(fd, atime, mtime, callback)的同步版本。返回undefined*

---

- **fs.utimes(path, atime, mtime, callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - atime `<number>` | `<string>` | `<Date>` 访问时间
  - mtime `<number>` | `<string>` | `<Date>` 修改时间
  - callback `<Function>`
    - err `<Error>`

  >*更改path指向对象的文件系统时间戳*

---

- **fs.utimesSync(path, atime, mtime)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - atime `<number>` | `<string>` | `<Date>` 访问时间
  - mtime `<number>` | `<string>` | `<Date>` 修改时间

  >*fs.utimes(path, atime, mtime, callback)的同步版本。返回undefined*

---

- **fs.link(existingPath, newPath, callback)**
  - existingPath `<string>` | `<Buffer>` | `<URL>` 指定需要被创建硬链接的文件的完整路径及文件名
  - newPath `<string>` | `<Buffer>` | `<URL>` 指定被创建的硬链接的完整路径及文件名
  - callback `<Function>`
    - err `<Error>`

  >*异步地创建硬连接。硬链接实际上是文件的一个或多个文件名。在操作系统中，一个文件被创建之后就拥有了个文件名，因此该文件的硬链接数量为1。但是我们可以通过某种特殊的操作为该文件在指定一个文件名，而这中特殊的操作称为对该文件创建硬链接。在对该文件创建一个硬链接之后，虽然表面上看起来拥有了两个不同的文件，但是在硬盘中这两个只不过是同一个文件的多个硬链接，**如果修改了一个文件中的内容，再打开另外一个文件，就会看见另一个文件中的内容也被修改了**。*

  ```javascript
  fs.link('./file/hello.txt', './file/hello_hard.txt', (err) => {
    if(err) throw err;
    fs.stat('./file/hello.txt', (err, stat) => {
      if(err) throw err;
      console.log(stat);
    })
  })
  ```
  
  ![](http://img.up-4ever.site/20201017232415.png)

  ![](http://img.up-4ever.site/20201017232432.png)

---

- **fs.unlink(path, callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - callback `<Function>`
    - err `<Error>`

  >*异步地删除文件或符号链接。 除了可能的异常，完成回调没有其他参数。fs.unlink() 对空或非空的目录均不起作用。 若要删除目录，则使用 fs.rmdir()。*

  ```javascript
  fs.unlink('./file/hello_hard.txt', (err) => {
    if(err) throw err;
    console.log('delete file successfully');
  })
  ```

---

- **fs.linkSync(existingPath, newPath)**
  - existingPath `<string>` | `<Buffer>` | `<URL>` 指定需要被创建硬链接的文件的完整路径及文件名
  - newPath `<string>` | `<Buffer>` | `<URL>` 指定被创建的硬链接的完整路径及文件名

  >*同步地创键硬连接。返回undefined*

---

- **fs.unlinkSync(path)**
  - path `<string>` | `<Buffer>` | `<URL>`

  >*同步地删除文件或符号链接。返回undefined*

---

- **fs.mkdir(path[, options], callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<Object>` | `<integer>`
    - recursive `<boolean>`
    - mode `<integer>`
  - callback `<Function>`
    - err `<Error>`

  >*异步地创建目录。*

  >*回调会传入可能的异常、以及创建的第一个目录的路径（如果 recursive 为 true）,(err, [path])。可选的options参数可以是整数（指定 mode（权限和粘滞位））、或对象（具有 mode 属性和 recursive 属性（指示是否要创建父目录））。当path是已存在的目录时，调用 fs.mkdir()仅在 recursive 为 false 时才会导致错误。*

---

- **fs.mkdirSync(path[, options])**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<Object>` | `<integer>`
    - recursive `<boolean>`
    - mode `<integer>`

  >*同步地创建目录。返回undefined，或者创建的第一个目录路径(如果recursive为true)。是fs.mkdir()的同步版本*

---

- **fs.rmdir(path[, options], callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<Object>`
    - maxRetries `<integer>` 如果遇到EBUSY、EMFILE、ENFILE、ENOTEMPTY 或 EPERM 错误，则 Node.js 会重试该操作(每次尝试时使用 retryDelay 毫秒时长的线性回退等待)。此选项表示重试的次数。如果 recursive 选项不为 true，则此选项会被忽略。默认值: 0。
    - recursive `<boolean>` 如果为 true，则执行递归的目录删除。默认值: false。
    - retryDelay `<integer>` 重试之间等待的时间(以毫秒为单位)。如果 recursive 选项不为 true，则此选项会被忽略。 默认值: 100。
  - callback `<Function>`
    - err `<Error>`

  >*异步地删除目录。对文件（而不是目录）使用 fs.rmdir() 会导致 ENOENT 错误（在 Windows 上）或 ENOTDIR 错误（在 POSIX 上）。*

---

- **fs.rmdirSync(path[, options])**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<Object>`
    - maxRetries `<integer>` 如果遇到EBUSY、EMFILE、ENFILE、ENOTEMPTY 或 EPERM 错误，则 Node.js 会重试该操作(每次尝试时使用 retryDelay 毫秒时长的线性回退等待)。此选项表示重试的次数。如果 recursive 选项不为 true，则此选项会被忽略。默认值: 0。
    - recursive `<boolean>` 如果为 true，则执行递归的目录删除。默认值: false。
    - retryDelay `<integer>` 重试之间等待的时间(以毫秒为单位)。如果 recursive 选项不为 true，则此选项会被忽略。 默认值: 100。

  >*同步地删除目录，返回undefined。对文件（而不是目录）使用 fs.rmdir() 会导致 ENOENT 错误（在 Windows 上）或 ENOTDIR 错误（在 POSIX 上）。*

---

- **fs.mkdtemp(prefix[, options], callback)**
  - prefix `<string>`
  - options `<string>` | `<Object>`
    - encoding `<string>` **默认值:** 'utf8'
  - callback `<Function>`
    - err `<Error>`
    - directory `<string>`

  >*异步地创建一个唯一的临时目录. **生成** 要附加在必需的prefix后面的六位随机字符，以创建唯一的临时目录.(就是生成六位随机字符，将这六位随机字符添加到prefix后面，以创建唯一的临时目录)*

  ```javascript
  console.log(path.join(os.tmpdir(), 'dir-'))
  //C:\Users\YANCHO~1\AppData\Local\Temp\dir-
  fs.mkdtemp(path.join(os.tmpdir(), 'dir-'), (err, directory) => {
    if(err) throw err;
    console.log(directory);
    //C:\Users\YANCHO~1\AppData\Local\Temp\dir-aX2EdU(这个就是创建的目录)
  });

  //如果要在指定的目录下创建临时目录
  const tmpDir = path.resolve(__dirname, './file');
  fs.mkdtemp(`${tmpDir}${path.sep}`, (err, directory) => {
    if(err) throw err;
    console.log(directory);
    //G:\node_learn\fs\file\CjOt1G
  })
  ```

  ![](http://img.up-4ever.site/20201017232457.jpg)

---

- **fs.mkdtempSync(prefix[, options])**
  - prefix `<string>`
  - options `<string>` | `<Object>`
    - encoding `<string>` **默认值:** 'utf8'

  >*同步地创建一个唯一的临时目录.返回创建的目录路径*

---

- **fs.open(path[, flag[, mode]], callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - flag `<string>` | `<number>` **默认值:** 'r'
  - mode `<string>` | `<number>` **默认值:** 0o666(可读写) [rw-rw-rw-]
  - callback `<Function>`
    - err `<Error>`
    - fd `<number>`
  
  >*异步地打开文件*

---

- **fs.openSync(path[, flags, mode])**
  - path `<string>` | `<Buffer>` | `<URL>`
  - flags `<string>` | `<number>` **默认值:** 'r'
  - mode `<string>` | `<number>` **默认值:** 0o666(可读写) [rw-rw-rw-]

  >*同步地打开文件夹，返回number,表示文件描述符*

---

- **fs.opendir(path[, options], callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<Object>`
    - encoding `<string>` | `<null>` **默认值:** 'utf8'
    - bufferSize `<number>` 当从目录读取时在内部缓冲的目录项的数量。值越高，则性能越好，但内存占用更高。**默认值:** 32
  - callback `<Function>`
    - err `<Error>`
    - dir `<fs.Dir>`

  >*异步地打开目录。创建一个fs.Dir类，其中包含所有用于更进一步读取和清理目录的的函数*

  ```javascript
  fs.opendir('./file', (err, dir) => {
  if(err) throw err;
  console.log(dir);
  dir.read().then((data) => {
      console.log(data);
    })
  });
  /*
  Dir {
  [Symbol(kDirHandle)]: DirHandle {},
  [Symbol(kDirBufferedEntries)]: [],
  [Symbol(kDirPath)]: './file',
  [Symbol(kDirClosed)]: false,
  [Symbol(kDirOptions)]: { bufferSize: 32, encoding: 'utf8' },
  [Symbol(kDirReadPromisified)]: [Function: bound [kDirReadImpl]],
  [Symbol(kDirClosePromisified)]: [Function: bound close]
  }
  Dirent { name: 'CjOt1G', [Symbol(type)]: 2 }
  */
  ```

---

- **fs.opendirSync(path[, options])**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<Object>`
    - encoding `<string>` | `<null>` **默认值:** 'utf8'
    - bufferSize `<number>` 当从目录读取时在内部缓冲的目录项的数量。值越高，则性能越好，但内存占用更高。**默认值:** 32

  >*同步地打开目录。创建一个fs.Dir类，其中包含所有用于更进一步读取和清理目录的的函数*

---

- **fs.read(fd, buffer, offset, length, position, callback)**
  - fd `<integer>`
  - buffer `<Buffer>` | `<TypedArray>` | `<DataView>`
  - offset `<integer>`
  - length `<integer>`
  - position `<integer>`
  - callback `<Function>`
    - err `<Error>`
    - bytesRead `<integer>`
    - buffer `<Buffer>`

  > - *从**fd**指定的文件中读取数据*。
  > - ***buffer**是数据（从fd读取）要被写入的 buffer*。
  > - ***offset**是buffer中开始写入的偏移量。*
  > - ***length** 是整数，指定要读取的字节数*
  > - ***position** 参数指定从文件中开始读取的位置，如果 position 为 null，则从当前文件位置读取数据，并更新文件位置。 如果 position 是整数，则文件位置会保持不变*
  > - *回调有三个参数 (err, bytesRead, buffer)。*

---

- **fs.readSync(fd, buffer, offset, length, position)**
  - fd `<integer>`
  - buffer `<Buffer>` | `<TypedArray>` | `<DataView>`
  - offset `<integer>`
  - length `<integer>`
  - position `<integer>`

  >*返回bytesRead的数量。fs.read的同步版本。*

---

- **fs.read(fd, [options,] callback)**
  - fd `<integer>`
  - options `<Object>`
    - buffer `<Buffer>` | `<TypedArray>` | `<DataView>` 默认值: Buffer.alloc(16384)
    - offset `<integer>` 默认值: 0
    - length `<integer>` 默认值: buffer.length
    - position `<integer>` 默认值: null
  - callback `<Function>`
    - err `<Error>`
    - bytesRead `<integer>`
    - buffer `<Buffer>`

  >*与上述fs.read函数类似，此版本带有一个可选的options对象。 如果未指定options对象，则使用上述默认值。*

  ```javascript
  fs.open('./file/test.txt', (err, fd) => {
    if(err) throw err;
    fs.read(fd, (err, bytesRead, buffer) => {
      if(err) throw err;
      console.log(bytesRead, buffer.toString());
    })
  })
  ```

  ![](http://img.up-4ever.site/20201017232529.jpg)

---

- **fs.readSync(fd, buffer, [options])**
  - fd `<integer>`
  - buffer `<Buffer>` | `<TypedArray>` | `<DataView>`
  - options `<Object>`
    - offset `<integer>` 默认值: 0
    - length `<integer>` 默认值: buffer.length
    - position `<integer>` 默认值: null
  
  >*返回bytesRead，即读取的字节数*

---

- ***fs.readv(fd, buffers[, position], callback)***
  - fd `<integer>`
  - buffers `<ArrayBufferView[]>
  - position `<integer>`
  - callback `<Function>`
    - err `<Error>`
    - bytesRead `<integer>`
    - buffers `<ArrayBufferView[]>

  >*从fd指定的文件中读取，并使用readv()写入ArrayBufferViews数组。position是从文件开头应读取数据的偏移量。 如果typeof position !== 'number'，将从当前位置读取数据。回调函数提供三个参数：err，bytesRead和buffers。 bytesRead是从文件读取的字节数。*
  
---

- ***fs.readvSync(fd, buffers[, position])***
  - fd `<integer>`
  - buffers `<ArrayBufferView[]>
  - position `<integer>`

  >*返回bytesRead，读取的字节数*

---

- **fs.readdir(path[, options], callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<string>` | `<Object>`
    - encoding `<string>` 默认值：'utf8'
    - withFileTypes `<boolean>` 默认值：false
  - callback `<Function>`
    - err `<Error>`
    - files `<string[]> | `<Buffer[]> | `<fs.Dirent[]>

  >*回调有两个参数 (err, files)，其中 files 是目录中文件的名称的数组（不包括 '.' 和 '..'）。如果 encoding 被设置为 'buffer'，则返回的文件名会作为 Buffer 对象传入。如果 options.withFileTypes 被设置为 true，则 files 数组会包含 fs.Dirent 对象。*

  ```javascript
  fs.readdir('./file', {encoding: 'utf8',withFileTypes:true}, (err, files) => {
    if(err) throw err;
    console.log(files);
    //withFileTypes:false时[ 'CjOt1G', 'hello.txt', 'hello_hard.txt', 'test.txt' ]
    //withFileTypes:true时
    //[
    //  Dirent { name: 'CjOt1G', [Symbol(type)]: 2 },
    //  Dirent { name: 'hello.txt', [Symbol(type)]: 1 },
    //  Dirent { name: 'hello_hard.txt', [Symbol(type)]: 1 },
    //  Dirent { name: 'test.txt', [Symbol(type)]: 1 }
    //]
  })
  ```

---

- **fs.readdirSync(path[, options])**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<string>` | `<Object>`
    - encoding `<string>` 默认值：'utf8'
    - withFileTypes `<boolean>` 默认值：false
  
  >*fs.readdir的同步版本*

---

- **fs.readFile(path[, options], callback)**
  - path `<string>` | `<Buffer>` | `<URL>` | `<integer>` 文件名或文件描述符
  - options `<Object>` | `<string>`
    - encoding `<string>` | `<null>` 默认值：null
    - flag `<string>` 默认值：'r'
  - callback `<Function>`
    - err `<Error>`
    - data `<Buffer>` | `<string>`

  >*异步地读取文件内容。如果 options 是字符串，则它指定字符编码。当路径是目录时，则 fs.readFile() 和 fs.readFileSync() 的行为是特定于平台的。 在 macOS、Linux 和 Windows 上，会返回错误。 在 FreeBSD 上，会返回目录内容的表示。*

  ```javascript
  fs.readFile('./file/test.txt', (err, data) => {
    if(err) throw err;
    console.log(data.toString());
  })
  ```

---

- **fs.readFileSync(path[, options])**
  - path `<string>` | `<Buffer>` | `<URL>` | `<integer>` 文件名或文件描述符
  - options `<Object>` | `<string>`
    - encoding `<string>` | `<null>` 默认值：null
    - flag `<string>` 默认值：'r'

  >*返回path的内容。fs.readFile的同步版本*

---

- ***fs.symlink(target, path[, type], callback)***
  - target  `<string>` | `<Buffer>` | `<URL>`
  - path  `<string>` | `<Buffer>` | `<URL>`
  - type  `<string>`
  - callback `<Function>`
    - err `<Error>`

  >*异步地创建符号链接。创建名为path的链接，该链接指向 target。(path -> target)。除了可能的异常，完成回调没有其他参数。type 参数仅在Windows上可用，在其他平台上则会被忽略。它可以被设置为 'dir'、 'file' 或 'junction'。如果未设置 type 参数，则 Node.js 将会自动检测 target 的类型并使用 'file' 或 'dir'。 如果 target 不存在，则将会使用 'file'。 Windows上的junction points要求target路径是绝对路径。 当使用 'junction' 时，target 参数将会自动地标准化为绝对路径。*

  ```javascript
  fs.symlink('./file/', './file_symlink', 'dir', (err) => {
    if(err) throw err;
  })
  ```

  >ps:这个在window 10上并没有创建成功。会报错：operation not permitted, symlink '.\file\' -> 'G:\node_learn\fs\file_symlink'。下面的同步版本也是一样。看样子是操作系统不允许。我想想...

---

- ***fs.symlinkSync(target, path[, type])***
  - target  `<string>` | `<Buffer>` | `<URL>`
  - path  `<string>` | `<Buffer>` | `<URL>`
  - type  `<string>`

  >*返回undefined。同步地创建符号链接*

---

- **fs.readlink(path[, options], callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<string>` | `<Object>`
    - encoding `<string>` 默认值：'utf8'
  - callback `<Function>`
    - err `<Errro>`
    - linkstring `<string>` | `<Buffer>`

  >*可选的 options 参数可以是字符串（指定字符编码）、或具有 encoding 属性（指定用于传给回调的链接路径的字符编码）的对象。 如果 encoding 被设置为 'buffer'，则返回的链接路径会作为 Buffer 对象传入。*

  ```javascript
  //应该这么写，上面的创建符号链接失败，这个就是demo吧，不是实操
  fs.symlink('./file', './file_symlink', 'dir', (err) => {
    if(err) throw err;
    fs.readlink('./file_symlink', (err, linkstring) => {
      if(err) throw err;
      console.log(linkstring);//  ./file
    })
  })
  ```

  >*链接路径linkstring应该是指 符号链接**所指向**的路径。*

---

- **fs.readlinkSync(path[, options])**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<string>` | `<Object>`
    - encoding `<string>` 默认值：'utf8'

  >*返回string或buffer*

---

- **fs.realpath(path[, options], callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<string>` | `<Object>`
    - encoding `<string>` 默认值：'utf8'
  - callback `<Function>`
    - err `<Error>`
    - resolvedPath `<string>` | `<buffer>`

  >*通过解析 .、 .. 和符号链接异步地计算规范路径名。仅支持可转换为 UTF8 字符串的路径。可选的 options 参数可以是字符串（指定字符编码）、或具有 encoding 属性（指定用于传给回调的路径的字符编码）的对象。 如果 encoding 被设置为 'buffer'，则返回的路径会作为 Buffer 对象传入。*

  ```javascript
  fs.realpath('./file', (err, resolvedPath) => {
    if(err) throw err;
    console.log(resolvedPath);//G:\node_learn\fs\file
  })
  ```

  >此函数的行为类似于 realpath(3)，但有一些例外:
  > - 在不区分大小写的文件系统上不执行大小写转换。

  > - 符号链接的最大数量与平台无关, 并且通常高于本地 realpath(3) 实现支持的数量。

---

- **fs.realpath.native(path[, options], callback)**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<string>` | `<Object>`
    - encoding `<string>` 默认值：'utf8'
  - callback `<Function>`
    - err `<Error>`
    - resolvedPath `<string>` | `<buffer>`

  >*异步的 [realpath(3)](http://man7.org/linux/man-pages/man3/realpath.3.html 'realpath(3)')。*

---

- **fs.realpathSync(path[, options])**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<string>` | `<Object>`
    - encoding `<string>` 默认值：'utf8'
  
  >*返回已解析的路径名。fs.realpath(path[, options], callback)的同步版本*

---

- **fs.realpathSync.native(path[, options])**
  - path `<string>` | `<Buffer>` | `<URL>`
  - options `<string>` | `<Object>`
    - encoding `<string>` 默认值：'utf8'

  >*同步地[realpath(3)](http://man7.org/linux/man-pages/man3/realpath.3.html 'realpath(3)')*

---

- **fs.rename(oldPath, newPath, callback)**
  - oldpath `<string>` | `<Buffer>` | `<URL>`
  - newpath `<string>` | `<Buffer>` | `<URL>`
  - callback `<Function>`
    - err `<Error>`

  >*异步地把 oldPath 文件重命名为 newPath 提供的路径名。 如果 newPath 已存在，则覆盖它。 除了可能的异常，完成回调没有其他参数。*

  ```javascript
  fs.rename('./file/hello.txt', './file/hello_bak.txt', (err) => {
    if(err) throw err;
    console.log('rename successfully');
  })
  ```

---

- **fs.renameSync(oldPath, newPath)**
  - oldpath `<string>` | `<Buffer>` | `<URL>`
  - newpath `<string>` | `<Buffer>` | `<URL>`

  >*返回undefined*

---

- **fs.write(fd, buffer[, offset[, length[, position]]], callback)**
  - fd `<integer>`
  - buffer `<Buffer>` | `<TypedArray>` | `<DataView>`
  - offset `<integer>`
  - length `<integer>`
  - position `<integer>`
  - callback `<Function>`
    - err `<Error>`
    - bytesWritten `<integer>`
    - buffer `<Buffer>` | `<TypedArray>` | `<DataView>`

  >*写入buffer到fd指定的文件。**offset**决定buffer中要被写入的部位， **length**是整数，指定要写入的字节数。position 指定文件开头的偏移量（数据要被写入的位置）。 如果 typeof position !== 'number'，则数据会被写入当前的位置。*

  ```javascript
  let data = Buffer.from('yangugu');
  fs.open('./file/new.txt', 'w', (err, fd) => {
    if(err) throw err;
    fs.write(fd, Buffer.from('yangugu'), 0, data.length - 2, (err, bytesWritten, buffer) => {
      if(err) throw err;
      console.log(bytesWritten, buffer);
    })
  })
  ```

  ![](http://img.up-4ever.site/20201017232604.png)

  ![](http://img.up-4ever.site/20201017232615.png)

---

- **fs.writeSync(fd, buffer[, offset[, length[, position]]])**
  - fd `<integer>`
  - buffer `<Buffer>` | `<TypedArray>` | `<DataView>`
  - offset `<integer>`
  - length `<integer>`
  - position `<integer>`

  >*返回写入的字节数。fs.write(fd, buffer[, offset[, length[, position]]], callback)的同步版本。*

---

- **fs.write(fd, string[, position[, encoding]], callback)**
  - fd `<integer>`
  - string `<string>`
  - position `<integer>`
  - encoding `<string>` 默认值：'utf8'
  - callback `<Function>`
    - err `<Error>`
    - written `<integer>`
    - string `<string>`

  >*将 string 写入到 fd 指定的文件。 如果 string 不是一个字符串，则会抛出异常。position 指定文件开头的偏移量（数据要被写入的位置）。 如果 typeof position !== 'number'，则数据会被写入当前的位置。encoding 是期望的字符串编码。回调会接收到参数 (err, written, string)，其中 written 指定传入的字符串中被要求写入的字节数。 被写入的字节数不一定与被写入的字符串字符数相同。*

  ```javascript
  let data = fs.readFileSync('./file/hello.txt');
  console.log(data);

  fs.open('./file/hello.txt', 'w', (err, fd) => {
    if(err) throw err;
    fs.write(fd, 'look', 2, 'utf8', (err, written, string) => {
      if(err) throw err;
      let data1 = fs.readFileSync('./file/hello.txt');
      console.log(data1);
    })
  })
  ```

  ![](http://img.up-4ever.site/20201017232638.jpg)
  
---

- **fs.writeSync(fd, string[, position[, encoding]])**
  - fd `<integer>`
  - string `<string>`
  - position `<integer>`
  - encoding `<string>` 默认值：'utf8'

  >*返回写入的字节数。fs.write(fd, string[, position[, encoding]], callback)的同步版本。*

---

- ***fs.writev(fd, buffers[, position], callback)***
  - fd `<integer>`
  - buffers `<ArrayBufferView[]>
  - position `<integer>`
  - callback `<Function>`
    - err `<Error>`
    - bufferWritten `<integer>`
    - buffers `<ArrayBufferView[]>

  >*使用 writev() 将一个 ArrayBufferView 数组写入 fd 指定的文件。其他字段含义参考fs.readv()。*

---

- ***fs.writevSync(fd, buffers[, position])***
  - fd `<integer>`
  - buffers `<ArrayBufferView[]>
  - position `<integer>`

  >*返回写入的字节数。fs.writev(fd, buffers[, position], callback)的同步版本。*

---

- **fs.writeFile(file, data[, options], callback)**
  - file `<string>` | `<Buffer>` | `<URL>` | `<integer>` 文件名或文件描述符
  - data `<string>` | `<Buffer>` | `<TypedArray>` | `<DataView>`
  - options `<Object>` | `<string>`
    - encoding `<string>` | `<null>` 默认值：'utf8'
    - mode `<integer>` 默认值：0o666
    - flag `<string>` 默认值：'w'
  - callback `<Function>`
    - err `<Error>`

  >*当file是文件名时，则异步地写入数据到文件(如果文件已存在，则覆盖文件。如果文件按不存在，则创建)。 data可以是字符串或buffer。*

  >*当 file 是文件描述符时，则其行为类似于直接调用 fs.write()。如果 options 是字符串，则它指定字符编码。*

  ```javascript
  //如果 data 是 buffer，则 encoding 选项会被忽略。
  const data = new Uint8Array(Buffer.from('Node.js 666'));
  fs.writeFile('./file/new.txt', data, (err) => {
    if(err) throw err;
    console.log('文件已保存');
  })
  ```

---

- **fs.writeFileSync(file, data[, options])**
  - file `<string>` | `<Buffer>` | `<URL>` | `<integer>` 文件名或文件描述符
  - data `<string>` | `<Buffer>` | `<TypedArray>` | `<DataView>`
  - options `<Object>` | `<string>`
    - encoding `<string>` | `<null>` 默认值：'utf8'
    - mode `<integer>` 默认值：0o666
    - flag `<string>` 默认值：'w'

  >*返回undefined。fs.writeFile(file, data[, options], callback)的异步版本。*

---

## fs的Promise API

fs.promises API 提供了一组备用的异步文件系统的方法，它们返回 Promise 对象而不是使用回调。 API 可通过 require('fs').promises 或 require('fs/promises') 访问。

### filehandler类

>*这一块的api和上面的提到的异步形式的api很像。如需要可以访问[fs的promise API](http://nodejs.cn/api/fs.html#fs_fs_promises_api)*

---

## [关于符号链接一些知识](https://www.zhihu.com/question/20729978 '符号连接、软连接、硬连接、快捷方式、副本(copy)之间有何区别和联系？')

>*其实只有两种：硬链接和符号链接。硬链接就是：一面墙上有一个洞，洞里放着一个苹果。从墙的这一面看是这个苹果，从墙的另一面看还是同一个苹果。硬链接就是同一块数据但有两个不同的名字，读写的时候本质上都是修改的同一块数据。硬链接在删除的时候只是删除了一个名字。只有一块数据的所有名字都删除了的时候，数据才会被删除。（删除的时候相当于把墙的这一面糊上，但苹果本身不动，只有两面都糊上了，你才会看不到）符号链接就是：我声称我有一个苹果，但是当你找我要的时候，我对你说，到某个建筑物的仓库就可以拿到那个苹果了。可见我并不真正拥有一个苹果，我只是拥有“某个地方有一个苹果”这个信息。但对于外部的观察者来说，这跟我实际上拥有一个苹果并无差异。符号链接的删除：假设我死了，不会影响到原始数据。假设原始数据没了，那我这个符号链接就变成了一张空头支票，也就是悬空的符号链接。快捷方式：本质上就是符号链接，只不过 Windows 的资源管理器可以直接管理这种文件。junction points：可能跟符号链接的区别仅仅是能否使用相对路径。复制：把一个苹果变成两个苹果。链接都是为文件起别名（alias）的方式，跟复制有本质区别的。*

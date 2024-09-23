---
title: node核心模块——os
date: '2020-05-27'
tags:
  - 前端开发
  - JavaScript
  - node
categories:
  - node
top_img: 'http://img.up-4ever.site/infinity-12769278.jpg'
cover: 'http://img.up-4ever.site/20201017192245.jpg'
abbrlink: 5fc11d9a
---
# node module: [os](http://nodejs.cn/api/os.html 'node中文官网os核心模块')

- **os.EOL**
  - string类型

    >*操作系统特定的行末标志。*
    >- 在POSIX上是  <kbd>\n</kbd>
    >- 在Windows上是 <kbd>\r\n</kbd>

    ```javascript
    console.log('hello' + os.EOL + 'world');
    ```

    ![](http://img.up-4ever.site/20201017192401.jpg)

    >*PS: \r: 回到当前行的最左边；\n: 向下移动一行，并不左右移动*

---

- **os.arch()**
  
  - 返回`<string>`

  >*返回为其编译 Node.js 二进制文件的操作系统的 CPU 架构。 可能的值有：'arm'、 'arm64'、 'ia32'、 'mips'、 'mipsel'、 'ppc'、 'ppc64'、 's390'、 's390x'、 'x32' 和 'x64'。返回的值等价于 process.arch。*

  ```javascript
  console.log(os.arch());// x64
  ```

---

- **os.constants**
  
  - Object类型

  >*包含错误码、进程信号等常用的操作系统特定的常量。*

---

- **os.cpus()**

  - 返回`<Object[]>`
  
  >*返回一个对象数组，其中包含有关每个逻辑 CPU 内核的信息。*

  ```javascript
  console.log(os.cpus());
  [
    {
      model: 'Intel(R) Core(TM) i7-8700 CPU @ 3.20GHz',
      speed: 3192, //以兆赫兹为单位
      times: {
        user: 18772359, //CPU 在用户模式下花费的毫秒数
        nice: 0, //CPU 在良好模式下花费的毫秒数
        sys: 16848062, //CPU 在系统模式下花费的毫秒数
        idle: 565596921, //CPU 在空闲模式下花费的毫秒数
        irq: 5211390 //CPU 在中断请求模式下花费的毫秒数
      }
    },
    ...
  ]
  ```

---

- **os.endianness()**
  - 返回`<string>`

  >*返回一个字符串，该字符串标识为其编译 Node.js 二进制文件的 CPU 的字节序。可能的值有， 'BE' 用于大端字节序， 'LE' 用于小端字节序。*

  ```javascript
  console.log(os.endianness()); //LE
  ```

---

- **os.totalmem()**
  - 返回`<integer>`

  >*以整数的形式返回系统的内存总量（以字节为单位）。*

---

- **os.freemem()**
  - 返回`<integer>`

  >*以整数的形式返回空闲的系统内存量（以字节为单位）*

---

- **os.getPriority([pid])**
  - pid `<integer>`要为其获取调度优先级的进程 ID。默认值0。
  - 返回`<integer>`

  >*返回由 pid 指定的进程的调度优先级。 如果未提供 pid 或者为 0，则返回当前进程的优先级.*

---

- **os.homedir()**
  - 返回`<string>`

  >*返回当前用户的主目录的字符串路径。*
  >- 在 POSIX 上，使用 $HOME 环境变量（如果有定义）。 否则，使用有效的 UID 来查找用户的主目录。
  >- 在 Windows 上，使用 USERPROFILE 环境变量（如果有定义）。 否则，使用当前用户的配置文件目录的路径。

---

- **os.hostname()**
  - 返回`<string>`

  >*以字符串的形式返回操作系统的主机名*

---

- **os.loadavg()**
  - 返回`<number[]>`

  >*返回一个数组，包含1、5和15分钟的平均负载。平均负载是系统活动性的测量，由操作系统计算得出，并表现为一个分数。平均负载是 UNIX 特定的概念。 在 Windows 上，其返回值始终为 [0, 0, 0]。*

---

- **os.networkInterfaces()**
  - 返回`<object>`

  >*返回一个对象，该对象包含已分配了网络地址的网络接口。*

  分配的网络地址的对象是上可用的属性包括：
  - address 分配的 IPv4 或 IPv6 地址
  - netmask IPv4 或 IPv6 的子网掩码
  - family  IPv4 或 IPv6
  - mac 网络接口的 MAC 地址
  - internal  如果网络接口是不可远程访问的环回接口或类似接口，则为 true，否则为 false
  - scopeid 数值型的 IPv6 作用域 ID（仅当 family 为 IPv6 时指定）
  - cidr  以 CIDR 表示法分配的带有路由前缀的 IPv4 或 IPv6 地址。如果 netmask 无效，则此属性会被设为 null

  ```javascript
  console.log(os.networkInterfaces());
  {
    ...
    'VMware Network Adapter VMnet1': [
      {
        address: 'fe80::9045:f2d4:5312:cd9a',
        netmask: 'ffff:ffff:ffff:ffff::',
        family: 'IPv6',
        mac: '00:50:56:c0:00:01',
        internal: false,
        cidr: 'fe80::9045:f2d4:5312:cd9a/64',
        scopeid: 86
      },
      {
        address: '169.254.205.154',
        netmask: '255.255.0.0',
        family: 'IPv4',
        mac: '00:50:56:c0:00:01',
        internal: false,
        cidr: '169.254.205.154/16'
      }
    ],
    ....
  }
  ```

---

- **os.platform()**
  - 返回`<string>`

  >*返回标识操作系统平台的字符串。 该值在编译时设置。 可能的值有 'aix'、 'darwin'、 'freebsd'、 'linux'、 'openbsd'、 'sunos' 和 'win32'。返回的值等价于process.platform*

---

- **os.release()**
  - 返回 `<string>`

  >*以字符串的形式返回操作系统发行版本*

  ```javascript
  console.log(os.release()); //6.1.7601
  ```

---

- **os.setPriority([pid, ]priority)**
  - pid `<integer>` 为其设置调度优先级的进程 ID。默认值 0
  - priority `<integer>` 分配给进程的调度优先级

  >*尝试为 pid 指定的进程设置调度优先级。 如果未提供 pid 或者为 0，则使用当前进程的进程 ID。*

  >*priority 输入必须是 -20（高优先级）到 19（低优先级）之间的整数。 由于 Unix 优先级和 Windows 优先级之间的差异， priority 会被映射到 os.constants.priority 中的六个优先级常量之一。 当检索进程的优先级时，此范围的映射可能导致 Windows 上的返回值略有不同。 为避免混淆，应将 priority 设置为优先级常量之一。*

  ```javascript
  //os.constants.priority
  priority: [Object: null prototype] {
      PRIORITY_LOW: 19,
      PRIORITY_BELOW_NORMAL: 10,
      PRIORITY_NORMAL: 0,
      PRIORITY_ABOVE_NORMAL: -7,
      PRIORITY_HIGH: -14,
      PRIORITY_HIGHEST: -20
    }
  ```

---

- **os.tmpdir()**
  - 返回`<string>`

  >*以字符串的形式返回操作系统的默认临时文件目录。这个在fs.mkdtemp(prefix[, options], callback)创建临时目录时曾有用到。*

---

- **os.type()**
  - 返回 `<string>`

  >*返回操作系统的名字。 例如，在 Linux 上返回 'Linux'，在 macOS 上返回 'Darwin'，在 Windows 上返回 'Windows_NT'。*

---

- **os.uptime()**
  - 返回 `<integer>`

  >*返回系统正常与运行的时间(以秒为单位)*

---

- **os.userInfo([options])**
  - options `<onject>`
    - encoding `<string>` 用于解释结果字符串的字符编码。如果将 encoding 设置为 'buffer'，则 username、 shell 和 homedir 的值将会是 Buffer 实例。默认值: 'utf8'。
  
  >*返回关于当前有效用户的信息。在POSIX平台上，这通常是密码文件的子集。返回的对象包含username、uid、gid、shell和homedir。在Windows上，则uid和 gid字段为-1，且shell为null。*

  ```javascript
  console.log(os.userInfo());
  {
    uid: -1,
    gid: -1,
    username: 'Ycq',
    homedir: 'C:\\Users\\Ycq',
    shell: null
  }
  ```

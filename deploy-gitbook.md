---
title: GitBook 部署至腾讯云服务器小记
date: '2023-09-29'
cover: 'http://img.up-4ever.site/infinity-1901105%20(1).jpg'
top_img: 'http://img.up-4ever.site/12.jpeg'
tags:
  - gitbook
categories:
  - 其他
abbrlink: 99cda666
---
# GitBook 部署至腾讯云服务器小记

## 背景

最近准备系统梳理一下技术体系，不计划以短文博客的形式进行，因此就想到了用 GitBook 来进行梳理。因此本文记录下GitBook 的安装以及部署到腾讯云服务器，并绑定到二级域名的一个过程以及其中遇到的问题。

如果你正在计划将 GitBook 部署到云服务器上，这篇文章或许可以帮助到你。

## 预期效果

因为之前搞过 Hexo 博客部署到腾讯云服务器，感觉 Hexo 插件提供的一键部署能力用着还不错，因此使用 GitBook 我预期的效果是

- 本地编写 markdown，本地预览
- 支持一键部署到腾讯云服务器
- 访问站点的二级域名可以访问到 GitBook 页面

下面具体说下过程（默认已安装 node 和 npm）。

## 部署环境

| 环境       |                |
| ---------- | :------------- |
| 本地环境   | macOS Monterey |
| 服务器环境 | centOS 7.1     |

## 云服务器配置

在做 Hexo 部署到腾讯云服务器时，相关的配置已经做过了，只是没有做记录，本次也顺便记录下。

> 默认认为读者是 git 基本操作是熟悉的，且本地已经生成相应的 ssh 公钥和私钥了（目的是为了免密登录 git 服务器）。
>
> 如不了解可参考文章：https://cloud.tencent.com/developer/article/1721430

### Git 服务器

> Github 是一个 git 服务器，我们自己在云服务器上也可以搭建一个自己的 git 服务器。

#### 安装

切换到 root 用户

```shell
su root
```

首先需要安装包的依赖

```shell
yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel
yum install  gcc perl-ExtUtils-MakeMaker
```

进入指定目录（这里选择 `usr/local/src`）选择最新版本 [Git](https://mirrors.edge.kernel.org/pub/software/scm/git/) （这里使用 `2.34.1`）下载并解压

```shell
cd /usr/local/src
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.34.1.tar.gz
tar -zxvf git-2.19.0.tar.gz
```

解压出来的 Git 文件都是源文件，我们需要进行编译，进入 Git 解压目录，进行编译和安装, 安装位置（这里选择 `usr/local/git`）, 整个过程可能需要一定等待......

```javascript
cd git-2.34.1
make prefix=/usr/local/git all
make prefix=/usr/local/git install
```

安装好后，我们需要配置环境变量，打开环境变量配置文件：

```shell
vim /etc/profile
```

在文件底部添加 Git 安装目录信息:

```shell
PATH=$PATH:/usr/local/git/bin
export PATH
```

> 其他方式（比如软链）也可以，只要是在环境变量的路径中可以找到 git 命令即可。
>
> 可执行 `echo $PATH` 查看当前的环境变量目录

然后刷新配置，使其生效

```shell
source /etc/profile
```

然后执行 `git --version` 即可查看版本号。

#### git 新用户及配置

创建 git 用户和密码

```shell
adduser git
passwd git
```

将 git 用户添加到 sudoers 文件中

```shell
chmod 740 /etc/sudoers
vim /etc/sudoers
```

在 `root    ALL=(ALL)       ALL` 下面一行添加 git 用户内容, 保存并退出

```bash
git      ALL=(ALL)       ALL
```

修改回 sudoers 文件权限

```shell
chmod 400 /etc/sudoers
```

#### 测试连接

将本地的 `ssh 公钥` 拷贝到云服务器，有两种方案：

**方案一（推荐）：执行下面命令**

```shell
ssh-copy-id -i ~/.ssh/id_rsa.pub git@server_ip
```

> server_ip 是云服务其的公网 IP

这个命令会默认在远端服务器 **git 用户**主目录下生成 `authorized_keys` 文件 （~/.ssh/authorized_keys）

方案二：手动拷贝

```shell
cd /home/git
mkdir .ssh
cd .ssh
touch authorized_keys
```

然后把本地公钥内容 copy 进去即可。

这样在执行 git 操作的时候就不需要输入密码了。

此时可以通过 ssh 命令(加上 `-v` 命令，可以输出详细 log)测试连接

```shell
ssh -v git@server_ip
```

### GitBook 目录（类似博客目录）

和博客目录保持一直，我们在 /home 目录下新建一个 book 目录来作为 GitBook 构建产物存放的目录：

```shell
mkdir book
```

赋予 git 用户权限（**重要**）

```shell
chown git:git -R /home/book
```

> 如果后面在本地想远程提交（push）时，报如下错误：
>
> ```
> error: insufficient permission for adding an object to repository database ./objects
> 
> fatal: failed to write object
> error: unpack failed: unpack-objects abnormal exit
> To ssh://<repo url>/<repo dir>
>  ! [remote rejected] master -> master (n/a (unpacker error))
> error: failed to push some refs to 'ssh://<repo url>/<repo dir>'
> ```
>
> 大概率是因为这个命令未执行。

#### Git hook 配置

指定目录位置（这里选择 git 用户主目录 `/home/git`）创建一个名为 `book.git` 的 `bare repo`

```bash
cd /home/git 
git init --bare blog.git
```

什么是 bare repo，以及 bare repo 的一些高级使用，可以看下面这三篇文章，下面的一些细节只是不做过多介绍

1. [Git Worktree 大法真香](https://dayarch.top/p/git-worktree-in-practice.html)
2. [Git Worktree 高级使用](https://dayarch.top/p/git-worktree-in-advance.html)
3. [一键生成Git Worktree 工作目录](https://dayarch.top/p/git-worktree-with-script.html)

同样赋予 git 用户相应权限(**重要**)

```shell
chown git:git -R blog.git
```

使用自动化配置，自然是要用到 Git 的钩子函数，这里我们需要 `post-receive`：

> `post-receive` 钩子在成功推送(push)操作之后被触发

```shell
vim blog.git/hooks/post-receive
```

然后添加如下内容(注意目录匹配)，保存并退出：

```bash
#!/bin/sh
git --work-tree=/home/book --git-dir=/home/git/book.git checkout -f
```

最后，赋予这个文件可执行权限(**重要**)

```shell
chmod +x /home/git/book.git/hooks/post-receive
```

至此，基本的准备工作就完成了，接下来我们需要处理 Nginx 的内容。

### nginx 安装及配置

同样切换到 root 用户，然后安装 nginx，并最终启动

```shell
su root
yum install -y nginx
systemctl start nginx.service
```

这时，在浏览器中输入公网 IP 地址，应该可以看到 Nginx 默认的欢迎界面。

然后打开 Nginx 的配置文件进行配置

```shell
server {
    listen       80;
    listen       [::]:80;
    server_name  book.<主域名>;
    root	     /home/book/book;
    location / {
        index index.html index.htm;		
    }
}
```

通过 `nginx -t` 命令进行检查，一切正常会输出如下结果：

```shell
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

重启 Nginx 服务

```shell
nginx -s reload
```

> 如需升级为 https，需要申请 ssl 证书。免费证书申请流程：https://cloud.tencent.com/document/product/400/6814
>
> ssl 证书的安装参考腾讯云的产品文档：https://cloud.tencent.com/document/product/400/35244

为了可以访问到二级域名，还需要加一条 DNS 解析

![](http://img.up-4ever.site/20230929184344.jpg)

这样访问二级域名就可以找到服务器 IP，然后 nginx 返回指定目录下的 index.html 文件（也就是 GitBook 的编译产物）。

## 本地安装 GitBook

> 建议全局安装

```shell
npm install -g gitbook-cli
```

在执行 `gitbook --version` 查看版本信息,执行时会自动安装 gitbook。

### 安装 GitBook 遇到的问题及解法

#### Error 1：TypeError: cb.apply is not a function

解法1：首先我们需要找到 gitbook 命令的具体位置：

```shell
which gitbook
# /usr/local/bin/gitbook
```

然后进入 bin 目录：

```shell
cd /usr/local/bin/gitbook
ll | grep gitbook
# gitbook -> ../lib/node_modules/gitbook-cli/bin/gitbook.js
```

然后进入 `../lib/node_modules/gitbook-cli` 目录

```shell
cd ../lib/node_modules/gitbook-cli
cd node_modules/graceful-fs
vim polyfills.js
```

修改 `ployfill.js` 文件（注释掉 Line 62-64）：

```js
// fs.stat = statFix(fs.stat)
// fs.fstat = statFix(fs.fstat)
// fs.lstat = statFix(fs.lstat)
```

解法2：降低 node 版本

因为我本地 node 版本为 18.x，需要切换到 10.x 的版本。

> 可以安装 node 的版本管理器，比如 [n](https://www.npmjs.com/package/n) 或者 [nvm](https://github.com/nvm-sh/nvm#intro)

```shell
n 10
```

切换到低版本后再执行 `gitbook --version`.

#### Error 2 Xcode 相关错误

比如：xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance；

这个错误可以忽略，没有影响安装。

## 本地 GitBook 配置

首先将 git 服务器上 book.git repo 克隆下来

```shell
git clone git@<server_ip>:/home/git/book.git
```

因为希望可以一键部署和本地开发预览，因此需要把相关命令添加到 `package.json` 中，因此在克隆下来的空目录中执行：

```shell
npm init --yes
```

在 `script` 中增加下面命令：

```json
"scripts": {
  "deploy": "gitbook build ./ book && npm run gam && npm run gp",
  "gam": "git add . && git commit -m 'deploy'",
  "gp": "git push origin master",
  "dev": "gitbook serve --port 9000"
},
```

然后我们就可以执行 `gitbook init` 命令，生成 `SUMMARY.md` 和 `README.md` 文件，执行 `npm run dev` 即可在本地预览，修改。

> gitbook-cli 命令可参考：https://juejin.cn/post/6844903591690371079 以及 https://juejin.cn/post/6931225754264928269 两篇文章。

修改完成后执行 `npm run deploy` 即可。

## 参考文章

- [Hexo博客部署到腾讯云服务器全过程（Nginx,证书,HTTPS）,你要的这里都有 ](https://www.cnblogs.com/FraserYu/p/15762299.html)
- [How I fixed a "cb.apply is not a function" error while using Gitbook](https://flaviocopes.com/cb-apply-not-a-function/)
- [GitBook使用教程](https://juejin.cn/post/6931225754264928269)
- [GitBook 使用教程](https://juejin.cn/post/6844903591690371079)
- ...


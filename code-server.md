---
title: 腾讯云服务器部署 code-server
date: '2022-09-17'
cover: 'http://img.up-4ever.site/infinity-4164366.jpg'
top_img: 'http://img.up-4ever.site/infinity-4164366.jpg'
tags:
  - code-server
categories:
  - Linux
abbrlink: b4572956
---
# 腾讯云服务器部署 code-server

## 背景

在2021年12月份，经历了人生第一次『毕业』，面临着马上要回家过年，希望在回家前可以把新工作确定下来，于是乎进入比较繁忙的准备面试和找工作的阶段，博客也就很少更新了。所幸在回家过年前找到了工作。

确定工作之后，计划写点什么，但是有不知道怎么去表达这个东西，所以就没了下文。后来有一天发现博客的图片全都成了裂图。后来才知道码云（Gitee）上托管的图片都加了防盗链；想起博客部署在github-page上打开速度又比较慢，因此在这种背景下决定自己重新搞一下博客，于是有了以下准备：

- 购买云服务器（腾讯云）
- 购买域名
- 申请七牛云图片存储空间（免费）

在工作之余慢慢的把博客做了迁移，之前的博客虽然没有下掉，但是也不再更新了。

之所以会部署 code-server 主要是之前就知道这个在线的VScode编辑器，但是苦于没有服务器一直没有搞（记得第一次在B站了解到这个东西的时候还想着可以用ipad写代码）。因此在博客迁移结束之后，就想着搞一把，爽一把。哈哈哈~（此处是杠铃般的笑声）

天也不早了，咱言归正传，说一下部署过程及遇到的问题和相应的解法。

冲冲冲~

## 部署步骤

### 部署环境

{% note green 'fa-solid fa-award' %}
Centos 7.x
{% endnote %}

### 下载 code-server

下载地址：[code-server](https://github.com/coder/code-server/releases)

或者命令行下载：

```shell
wget https://github.com/coder/code-server/releases/download/v3.5.0/code-server-3.5.0-linux-x86_64.tar.gz
```

### 解压安装

```shell
tar -zxvf code-server-3.5.0-linux-x86_64.tar.gz -C /opt/
cd /opt
mv code-server-3.5.0-linux-x86_64 code-server
```

### 配置 code server

```shell
cd code-server
mkdir data extensions
vim config.yaml  # 创建并编辑配置文件
```

编辑配置文件

```shell
bind-addr: 127.0.0.1:8080
auth: password
password: <Your passwd>
cert: false
user-data-dir: /opt/code-server/data/
extensions-dir: /opt/code-server/extensions/
```

### 启动 code-server

code-server 命令的路径是`/opt/code-server/bin`, 为了启动方便，我们把这个路径加到环境变量中：

```shell
vim ~/.bashrc
```

在文件的最后一行添加：

```shell
export PATH=/opt/code-server/bin:$PATH
```

然后执行`source ~/.bashrc`使配置生效。

然后在后台运行code-server：

```shell
code-server --config /opt/code-server/config.yaml &
```

这个时候其实code-server已经启动成功，下面需要在访问子域名`xxx.up-4ever.site`的时候打开这个在线vscode。

### 配置 nginx && 配置DNS解析

按照上面的说法，我们需要在访问`xxx.up-4ever.site`时候可以打开线上的vscode，为了实现这个效果，需要做两件事请：

- 在腾讯云控制台配置子域名解析到服务器
- nginx转发来之子域名的请求

1. 首先添加一条记录：子域名解析到云服务器

![](http://img.up-4ever.site/20220917225544.png)

2. 配置nginx

```shell
server {
    listen	80;
    listen	[::]:80;
    server_name	xxx.up-4ever.site;
    location / {
        proxy_pass http://127.0.0.1:8080;
    }
}
```

然后打开`xxx.up-4ever.site` 发现输入密码后，一直白屏，打开浏览器控制台发现报错如下：WSS，WebSocket close with status code 1006，Unable to read file。

分析发现每一个错误都跟websocket有关，而最有效的信息便是 1006 这个错误码。在websocket中，1006就是超时的意思，再进一步查阅资料发现是因为我们对地址进行了代理转发而引起的，原因是在代理时没有对websocket进行代理。解决办法：利用Upgrade协议头机制将连接从HTTP连接升级到WebSocket连接。

因此需要修改nginx配置如下

```shell
server {
    listen	80;
    listen	[::]:80;
    server_name	xxx.up-4ever.site;
    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_redirect     off;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header REMOTE-HOST $remote_addr;
        proxy_set_header   Upgrade          $http_upgrade;
        proxy_set_header   Connection       "Upgrade";
        proxy_connect_timeout      60;
        proxy_send_timeout         90;
        proxy_read_timeout         90;
        proxy_buffer_size          4k;
        proxy_buffers              4 32k;
        proxy_busy_buffers_size    64k;
        proxy_temp_file_write_size 64k;
        add_header X-Cache $upstream_cache_status;
        add_header Cache-Control no-cache;
    }
}
```

然后重启nginx即可。

```shell
nginx -t
nginx -s reload
```

然后打开`xxx.up-4ever.site`就可以正常打开在线Vscode咯~

## code-server 配置参数

| 参数                | 说明                                                         |
| :------------------ | :----------------------------------------------------------- |
| auth                | 自定义身份验证类型，如果不设置则默认只有password。[password, none] |
| cert                | https证书路径。如果没有提供路径，则自动生成。                |
| cert-key            | 非生成证书时证书密钥的路径，如果用自己的https证书认证的话此段必填。 |
| disable-updates     | 禁用更新，没什么好说的。                                     |
| disable-telemetry   | 禁用遥测。就是不允许向微软服务器发送错误或数据信息。         |
| help                | 帮助指令。                                                   |
| open                | 启动时在浏览器中打开。不能远程工作。                         |
| bind-addr           | 设置ip地址访问与端口号。[host:port ]                         |
| socket              | socket路径，设置bing-addr的话此指令可以忽略。                |
| version             | 查看当前版本。                                               |
| user-data-dir       | 用户文件路径。                                               |
| extensions-dir      | 扩展文件存储路径。                                           |
| list-extensions     | 列出vscode安装的所有扩展插件。                               |
| force               | 阻止在安装VS代码扩时显示提示 。                              |
| install-extension   | 通过id或者vsix安装指定vscode扩展插件。                       |
| uninstall-extension | 通过id卸载指定vscode扩展插件。                               |
| show-versions       | 显示vscode扩展插件版本。                                     |
| proxy-domain        | 设置代理端口的域名。                                         |
| verbose             | 启用详细日志记录。                                           |

## 参考文章

- [CentOS7.6 安装在线vscode——code-server](http://www.vleity.com/detail/gVYkF3QvpSSErkEzW9c4uL.html)
- [浏览器上的IDE：code-server安装——服务器版的vscode](https://www.codenong.com/cs105990565/)
- [部署code-server并通过域名访问（内含实战填坑）](https://www.bilibili.com/read/cv14047073)

---
title: 关于linux丢失/root文件夹导致的-bash-4.1#(不显示路径)的解决办法
date: '2018-04-13'
tags:
  - Linux
categories:
  - CSS
cover: 'http://img.up-4ever.site/20210420224048.jpeg'
top_img: 'http://img.up-4ever.site/infinity-2875549.jpg'
abbrlink: c3fdd285
---
# 关于linux丢失/root文件夹导致的-bash-4.1#(不显示路径)的解决办法

1. 新建root目录

```sh
mkdir /root
```
2. 重新从主默认文件拷贝配置信息到/root目录下

```sh
cp /etc/skel/.bash_profile /root/
cp /etc/skel/.bashrc /root/
```
3. 注销后重新登录

```sh
logout
su - root
```
重新登录后应该就可以正常显示啦！

## 参考文章

- [https://www.cnblogs.com/2lianzhang/p/8052312.html](https://www.cnblogs.com/2lianzhang/p/8052312.html)
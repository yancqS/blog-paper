---
title: 转载-节选-科技爱好者周刊节选（172期）
date: '2021-08-20'
cover: 'http://img.up-4ever.site/20210820154332.jpeg'
top_img: 'http://img.up-4ever.site/infinity-1377518.jpg'
tags:
  - 趣谈
  - 转载
categories:
  - 其他
abbrlink: 5b36a5f8
---
# 科技爱好者周刊（第 172 期）：我们会死于气候灾难吗？

## 前言

本文节选自阮一峰老师的《科技爱好者周刊》第172期。

《周刊》链接：[科技爱好者周刊172期](https://www.ruanyifeng.com/blog/2021/08/weekly-issue-172.html)

## 正文

*科技动态* 部分：

- [音乐消费量下降](https://www.eurekalert.org/news-releases/924367)

![](http://img.up-4ever.site/20210820154332.jpeg)

一项研究发现，自从新冠疫情爆发以后，音乐流媒体的消费量大约下降了10%，使得全球最大音乐网站之一的 Spotify 收入减少。

这很奇怪，人们在隔离期间无事可做，反而减少了听音乐。

原因可能是，音乐并不是独立的娱乐活动，而往往是对其他活动（比如上下班期间）的补充。一旦人们减少了其它活动，音乐消费也随之减少。

- [酒驾监控系统](https://www.vice.com/en/article/dyvk9z/every-car-made-after-2027-may-have-drunk-driving-monitoring-system)

最近，美国国会通过一项法律，规定2027年后生产的所有汽车都必须配备酒驾监控系统，防止驾驶员酒后驾车。美国每年因为酒后驾车，有超过10000人丧生。

![](http://img.up-4ever.site/20210820154528.jpeg)

但是，麻烦的是目前还没有任何技术，能够主动发现酒后驾车。美国政府正在研究两种有希望的方案。一种是检测车内空气，找出空气里面的酒精成分，这种方法的问题是，无法区分酒精来自驾驶员的呼吸，还是来自乘客的呼吸。

另一种方案是在引擎按钮上，装备一个红外线触摸传感器，通过皮肤检测血液中的酒精含量。

---

*文章* 部分：

- [MDN 如何实现自动补全搜索？](https://hacks.mozilla.org/2021/08/mdns-autocomplete-search/)（英文）

![](http://img.up-4ever.site/20210820160257.jpeg)

本文介绍 Mozilla 的文档网站如何实现搜索框的自动补全功能。

> 里面用到了flexSearch第三方库，这个库在我的博客有用到，但是这个库有一个问题就是不能检索中文。如果有哪位大佬知道比较好用的中英文检索工具可以评论区赐教，小弟先行谢过~~

> 总得来说，这篇文章是值得一读的。

- [Git 的两个新命令](https://www.banterly.net/2021/07/31/new-in-git-switch-and-restore/)（英文）

![](http://img.up-4ever.site/20210820164329.jpeg)

新版本的 Git 加了两个命令：git switch 和 git restore，本文介绍它们的用法。

git switch 用来切换分支，git restore 用来恢复原样，可以取代 git checkout。

---

*工具* 部分

- [static.wiki](https://github.com/segfall/static-wiki)

![](http://img.up-4ever.site/20210820165242.jpeg)

维基百科的静态化工具，将维基百科变成一个只读的离线静态网站。

它将所有条目导入一个 SQLite 数据库，然后实现一个前端的查询展示页面。注意，它的查询是 SQLite 的静态化查询，不需要有后端服务。

> 有趣有趣

---

*资源* 部分：

- [自己动手写编译器](https://github.com/pandolia/tinyc)

一本中文电子书，介绍一个非常简单的编译器的实现，通过实例，描述基本的编译原理及过程。

---

*图片* 部分：

- [阿富汗地形图](https://www.visualcapitalist.com/map-explainer-key-facts-about-afghanistan/)

下面是阿富汗的地形图。

![](http://img.up-4ever.site/20210820170824.jpeg)

可以看到，这个国家全境都是高山，只有西南方向有一点点平原。而且，它东部连接帕米尔高原，海拔非常高，最高峰将近7500米，到处是高耸的雪山、深邃的山谷和无人的高原荒漠。

这种地形，再加上没有高等级道路，特别适合游击队出没，不适合正规军作战，大型武器无法发挥威力。这就是前苏联和美国都无法在阿富汗获胜，最终只能铩羽而归的主要外部原因。

---

*言论* 部分：

- Windows 的游戏优势有多大？根据 Steam 游戏平台 2021 年 6 月的数据，Linux 上玩家比例是 0.89%，macOS 是 2.54%，Windows 是 96.57%。		-- [phoronix.com](https://www.phoronix.com/scan.php?page=news_item&px=Steam-On-Linux-Tap-Dance-0.9)

## 最后

感谢阮一峰老师的《周刊》

love & peace :kissing_heart:

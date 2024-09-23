---
title: Jetbrains 全家桶激活
description: Jetbrains 全家桶激活
date: '2023-08-13'
cover: 'http://img.up-4ever.site/infinity-1140607.jpg'
top_img: 'http://img.up-4ever.site/infinity-1140607.jpg'
tags:
  - 转载
categories:
  - 经验分享
abbrlink: 7a9026b0
---
# Jetbrains 全家桶激活

## 前言

本文转载自[Jetbrains 全家桶激活](https://zgao.top/jetbrains-%E5%85%A8%E5%AE%B6%E6%A1%B6%E6%BF%80%E6%B4%BB/)。感谢作者**Zgao**的分享！

## 正文

互联网上有很多公开的开放Jetbrains激活服务器。利用这些服务器就可以直接激活Jetbrains的所有产品。

### Shodan 搜索语法

https://www.shodan.io/search?query=Location%3A+https%3A%2F%2Faccount.jetbrains.com%2Ffls-auth

![](http://img.up-4ever.site/20230813183019.png)

### 激活 IDE

随便找一个ip测试激活。

![](http://img.up-4ever.site/20230813183102.png)

![](http://img.up-4ever.site/20230813183149.png)
![](http://img.up-4ever.site/20230813183150.png)

也可以使用shodan api来批量获取。

```shell
shodan search 'Location: https://account.jetbrains.com/fls-auth' --fields ip_str,port | awk  '{if($2==443){print "https://"$1}else{print "http://"$1":"$2}}'
```

> shodan cli 的搜索好像对账号有要求。（非原文）

个别不能用的话，换个ip就行了。这个小技巧可以节省大量的时间每次去找jetbrains全家桶的破解版或者各种激活码！

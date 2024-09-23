---
title: Xshell无法连接到Linux虚拟机
date: '2018-10-21'
tags:
  - Linux
  - Xshell
cover: 'http://img.up-4ever.site/20210420224048.jpeg'
categories:
  - Linux
top_img: 'http://img.up-4ever.site/infinity-4164366.jpg'
abbrlink: 720b8105
---
# Xshell无法连接到Linux虚拟机

今天用Xshell连接Linux虚拟机时，连接失败（检测了sshd服务是打开的）。不知道为啥。于是就用虚拟机ping了主机，没问题，可以ping通，但是呢，用主机ping虚拟机，不好使。于是在网上找了找，找到了答案。

原文地址：[linux虚拟机能ping通windows主机，windows主机ping不通linux虚拟机的解决办法](https://blog.csdn.net/clean_water/article/details/53023308)

下面的我根据个人情况有一点修改。

## 虚拟机网络连接方式选择NAT

![](http://img.up-4ever.site/20210420204551.png)

## 关闭liunx防火墙

关闭防火墙的命令：

```sh
service iptables stop
```

确定是否关闭：`service iptables status`

![](http://img.up-4ever.site/20210420204806.png)

## 关闭windows防火墙

>可自行百度

## 查看虚拟机IP

在linux使用ifconfig查看ip，如图可看出ip是192.168.19.130

![](http://img.up-4ever.site/20210420205058.png)

在windows主机的网络设置中选择VMNet8

![](http://img.up-4ever.site/20210420205129.png)

——>右键——>属性——>TCP/Ipv4——>IP地址输入192.168.19.1（这个是根据虚拟机192.168.19.130来的），子网掩码输入255.255.255.0，其他空

![](http://img.up-4ever.site/20210420205229.png)

再互相ping试试，应该就可以了

主机ping虚拟机：

![](http://img.up-4ever.site/20210420205305.png)

虚拟机ping主机

![](http://img.up-4ever.site/20210420205341.png)

然后连接Xshell，ok啦。

## 参考文章

- [linux虚拟机能ping通windows主机，windows主机ping不通linux虚拟机的解决办法](https://blog.csdn.net/clean_water/article/details/53023308)
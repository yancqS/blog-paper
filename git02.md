---
title: Git 基础（第二章）
date: '2024-09-25'
cover: 'http://img.up-4ever.site/infinity-688645.jpg'
top_img: 'http://img.up-4ever.site/infinity-688645.jpg'
tags:
  - Git
categories:
  - Git
abbrlink: 30cd1f9
---

# Git 基础

## 获取 Git 仓库

1. 将尚未进行版本控制的本地目录转换为 Git 仓库;
```shell
git init
```
2. 从其它服务器 克隆 一个已存在的 Git 仓库。
```shell
git clone <url>
```

## 记录每次更新到仓库

工作目录下的每一个文件都不外乎这两种状态:**已跟踪** 或 **未跟踪**。已跟踪的文件就是 Git 已经知道的文件，工作目录中除已跟踪文件外的其它所有文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有被放 入暂存区。

![iwEcAqNwbmcDAQTRBg0F0QKpBrBef0RFHmcSDgbaoCn8GssAB9MAAAAAogUD3ggACaJpbQoAC9IAAYT0.png_720x720q90](http://img.up-4ever.site/20240924203604.jpg)

## 检查当前文件状态

**可以用 `git status` 命令查看哪些文件处于什么状态。**

在克隆仓库后立即使用此命令，会看到类似这样的 输出:

```
$ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  nothing to commit, working directory clean
```

在项目下创建一个新的 README 文件。 如果之前并不存在这个文件，使用 git status 命令，你 将看到一个新的未跟踪文件:

```
$ echo 'My Project' > README
$ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Untracked files:
    (use "git add <file>..." to include in what will be committed)
      README
  nothing added to commit but untracked files present (use "git add" to track)
```

在状态报告中可以看到新建的 README 文件出现在 Untracked files 下面。 未跟踪的文件意味着 Git 在之前 的快照(提交)中没有这些文件;Git 不会自动将之纳入跟踪范围，除非你明明白白地告诉它“我需要跟踪该文 件”。 这样的处理让你不必担心将生成的二进制文件或其它不想被跟踪的文件包含进来。

## 跟踪新文件

**使用命令 `git add` 开始跟踪一个文件。**


---
title: progit 第二章读书笔记
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

### 检查当前文件状态

**可以用 `git status` 命令查看哪些文件处于什么状态。**

在克隆仓库后立即使用此命令，会看到类似这样的 输出:

```shell
$ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  nothing to commit, working directory clean
```

在项目下创建一个新的 README 文件。 如果之前并不存在这个文件，使用 git status 命令，你 将看到一个新的未跟踪文件:

```shell
$ echo 'My Project' > README
$ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Untracked files:
    (use "git add <file>..." to include in what will be committed)
      README
  nothing added to commit but untracked files present (use "git add" to track)
```

在状态报告中可以看到新建的 README 文件出现在 **Untracked files** 下面。 未跟踪的文件意味着 Git 在之前 的快照(提交)中没有这些文件;Git 不会自动将之纳入跟踪范围，除非你明明白白地告诉它“我需要跟踪该文 件”。 这样的处理让你不必担心将生成的二进制文件或其它不想被跟踪的文件包含进来。

### 跟踪新文件

**使用命令 `git add` 开始跟踪一个文件。**所以，要跟踪 README 文件，运行:

```shell
git add README
```

此时再运行git status命令，会看到README文件已被跟踪，并处于暂存状态:

```shell
$ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes to be committed:
    (use "git restore --staged <file>..." to unstage)
      new file:   README
```

只要在 **Changes to be committed** 这行下面的，就说明是已暂存状态。**git add 命令使用文件或目录的路径作为参数;如果参 数是目录的路径，该命令将递归地跟踪该目录下的所有文件。**

### 暂存已修改的文件

现在我们来修改一个已被跟踪的文件。 如果你修改了一个名为 CONTRIBUTING.md 的已被跟踪的文件，然后运 行git status命令，会看到下面内容:

```shell
$ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)
      new file:   README
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)
      modified:   CONTRIBUTING.md
```

文件CONTRIBUTING.md出现在**Changes not staged for commit**这行下面，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。 要暂存这次更新，需要运行 git add 命令。

> `git add` 是个多功能命令:可以用它开 始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。 将这 个命令理解为**“精确地将内容添加到下一次提交中”**而不是“将一个文件添加到项目中”要更加合适。

当你允许 `git add` 将 CONTRIBUTING.md 放到暂存区后，再次编辑 CONTRIBUTING.md 文件后，再次 `git status` 后看到：

```shell
$ vim CONTRIBUTING.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    new file:   README
    modified:   CONTRIBUTING.md
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
    modified:   CONTRIBUTING.md
```

你会发现 CONTRIBUTING.md 文件同时出现在暂存区和非暂存区。这是因为 Git 只 不过暂存了你运行 git add 命令时的版本。如果你现在提交(commit)，CONTRIBUTING.md 的版本是你最后一次运行 git add 命令时的那个版本，而不是你运行 git commit 时，在**工作目录**中的当前版本。 所以，运行了 git add之后又作了修订的文件，需要重新运行git add把最新版本重新暂存起来。

### 状态简览

`git status`命令的输出十分详细，但其用语有些繁琐。可以使用 `git status -s` 命令或 `git status --short` 命令，你将得到一种 格式更为紧凑的输出。

```shell
$ git status -s
   M README
  MM Rakefile
  A  lib/git.rb
  M  lib/simplegit.rb
  ?? LICENSE.txt
```

输出中有两栏，左栏指明了暂存区的状态，右栏指明了工作区的状态。

新添加的未跟踪文件前面有 ?? 标记，新添加到暂存区中的文件前面有 A 标记，修改过的文件前面有 M 标记。

例如：上面的状态报告显示: README 文件在工作区已修改但尚未暂存，而 lib/simplegit.rb 文件已修改且已暂存。 Rakefile 文件已修改，暂存后又作了修改，因此该文件的修改中既有已暂存的部分，又有未暂存的部分。

### 忽略文件

可以创建一个名为 `.gitignore` 的文件，列出要忽略的文件的模式。

文件 .gitignore 的格式规范如下:

- 所有空行或者以 # 开头的行都会被 Git 忽略。
- 可以使用标准的 glob 模式匹配，它会递归地应用在整个工作区中。
- 匹配模式可以以(/)开头防止递归。
- 匹配模式可以以(/)结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上叹号(!)取反。

> 所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。 星号(*)匹配零个或多个任意字符;[abc] 匹配 任何一个列在方括号中的字符 (这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c); 问号(?)只 匹配一个任意字符;如果在方括号中使用短划线分隔两个字符， 表示所有在这两个字符范围内的都可以匹配 (比如 [0-9] 表示匹配所有 0 到 9 的数字)。 使用两个星号(**)表示匹配任意中间目录，比如 a/**/z 可以 匹配 a/z 、 a/b/z 或 a/b/c/z 等。

看一个 .gitignore 文件的例子

```shell
# 忽略所有的 .a 文件
*.a
# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a
# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO
# 忽略任何目录下名为 build 的文件夹
build/
# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt
# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf

```

### 查看已暂存和未暂存的修改

如果git status命令的输出对于你来说过于简略，而你想知道具体修改了什么地方，可以用git diff命令。

要查看尚未暂存的文件更新了哪些部分，不加参数直接输入 `git diff`，此命令比较的是**工作目录中当前文件和暂存区域快照之间**的差异。 也就是修改之后还没有暂存起来的变化内容。

若要查看已暂存的将要添加到下次提交里的内容，可以用 `git diff --staged` 命令。 这条命令将比对**已暂存文件与最后一次提交的文件**差异。

### 提交更新

现在的暂存区已经准备就绪，可以提交了。运行提交命令 `git commit` 这样会启动你选择的文本编辑器来输入提交说明。

也可以在 commit 命令后添加 -m 选项，将提交信息与命令放在同一行，如下所示:

```shell
$ git commit -m "message for this commit"
[master 463dc4f] Story 182: Fix benchmarks for speed
  2 files changed, 2 insertions(+)
  create mode 100644 README
```

可以看到，提交后它会告诉你，当前是在哪个分支(master)提交的，本 次提交的完整 SHA-1 校验和是什么(463dc4f)，以及在本次提交中，有多少文件修订过，多少行添加和删改 过。

请记住，提交时记录的是放在暂存区域的快照。 任何还未暂存文件的仍然保持已修改状态，可以在下次提交时纳入版本管理。 每一次运行提交操作，都是对你项目作一次快照，以后可以回到这个状态，或者进行比较。

### 跳过使用暂存区

Git 提供了一个跳过使用暂 存区域的方式， 只要在提交的时候，给 git commit 加上 -a 选项，Git 就会自动把所有已经跟踪过的文件暂存 起来一并提交，从而跳过git add步骤。

```shell
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
    modified:   CONTRIBUTING.md
no changes added to commit (use "git add" and/or "git commit -a")
$ git commit -a -m 'added new benchmarks'
[master 83e38c7] added new benchmarks
 1 file changed, 5 insertions(+), 0 deletions(-)
```

### 移动文件

在 Git 中对文件改名，可以这么做:

```shell
git mv file_from file_to
```

它会恰如预期般正常工作。 实际上，即便此时查看状态信息，也会明白无误地看到关于重命名操作的说明:

```shell
$ git mv README.md README
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)
    renamed:    README.md -> README
```

其实，运行git mv就相当于运行了下面三条命令:

```shell
mv README.md README
git rm README.md
git add README
```

如此分开操作，Git 也会意识到这是一次重命名，所以不管何种方式结果都一样。 两者唯一的区别在于，git mv 是一条命令而非三条命令，直接使用 git mv 方便得多。 不过在使用其他工具重命名文件时，记得在提交前 git rm删除旧文件名，再git add添加新文件名。

## 查看提交历史

回顾下提交历史可以通过`git log` 命令来完成：

```shell
$ git log
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700
    changed the version number
commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700
    removed unnecessary test
commit a11bef06a3f659402fe7563abf99ad00de2209e6
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 10:31:28 2008 -0700
first commit
```

不传入任何参数的默认情况下，git log 会按时间先后顺序列出所有的提交，最近的更新排在最上面。正如你 所看到的，这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明。

### 选项参数

- `-p` 或 `--patch`:显示每次提交所引入的差异(按 补丁 的格式输出)。也可以限制显示的日志条目数量，例如使用 -2 选项来只显示最近的两次提交:

```shell
$ git log -p -2
commit ca82a6dff817ec66f44342007202690a93763949
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Mon Mar 17 21:52:11 2008 -0700
    changed the version number
diff --git a/Rakefile b/Rakefile
index a874b73..8f94139 100644
--- a/Rakefile
+++ b/Rakefile
@@ -5,7 +5,7 @@ require 'rake/gempackagetask'
 spec = Gem::Specification.new do |s|
     s.platform  = Gem::Platform::RUBY
     s.name      = "simplegit"
-    s.version   = "0.1.0"
+    s.version   = "0.1.1"
     s.author    = ""Scott Chacon"
     s.email     = "schacon@gee-mail.com"
     s.summary   = "A simple gem for using Git in Ruby code."
commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
Author: Scott Chacon <schacon@gee-mail.com>
Date:   Sat Mar 15 16:40:33 2008 -0700
    removed unnecessary test
diff --git a/lib/simplegit.rb b/lib/simplegit.rb
index a0a60ae..47c6340 100644
--- a/lib/simplegit.rb
+++ b/lib/simplegit.rb
@@ -18,8 +18,3 @@ class SimpleGit
     end
	end -
-if $0 == __FILE__
-  git = SimpleGit.new
-  puts git.show
-end
```

- `--stat`: 查看每次提交的简单统计信息

```shell
$ git log --stat
  commit ca82a6dff817ec66f44342007202690a93763949
  Author: Scott Chacon <schacon@gee-mail.com>
  Date:   Mon Mar 17 21:52:11 2008 -0700
      changed the version number
   Rakefile | 2 +-
   1 file changed, 1 insertion(+), 1 deletion(-)
  commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
  Author: Scott Chacon <schacon@gee-mail.com>
  Date:   Sat Mar 15 16:40:33 2008 -0700
      removed unnecessary test
   lib/simplegit.rb | 5 -----
   1 file changed, 5 deletions(-)
  commit a11bef06a3f659402fe7563abf99ad00de2209e6
  Author: Scott Chacon <schacon@gee-mail.com>
  Date:   Sat Mar 15 10:31:28 2008 -0700
			first commit
   README           |  6 ++++++
   Rakefile         | 23 +++++++++++++++++++++++
   lib/simplegit.rb | 25 +++++++++++++++++++++++++
   3 files changed, 54 insertions(+)
```

- `--pretty`: 可以使用不同于默认格式的方式展示提交历史，比如 oneline 会将每个提交放在一行显示，在浏览大量的提交时非常有用：

```shell
$ git log --pretty=oneline
ca82a6dff817ec66f44342007202690a93763949 changed the version number
085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7 removed unnecessary test
a11bef06a3f659402fe7563abf99ad00de2209e6 first commit
```

最有意思的是 format ，可以定制记录的显示格式:

```shell
$ git log --pretty=format:"%h - %an, %ar : %s"
ca82a6d - Scott Chacon, 6 years ago : changed the version number
085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
a11bef0 - Scott Chacon, 6 years ago : first commit
```

git log --pretty=format 常用的选项如下：

| 选项 |                     说明                     |
| ---- | :------------------------------------------: |
| %H   |               提交的完整哈希值               |
| %h   |               提交的简写哈希值               |
| %T   |                树的完整哈希值                |
| %t   |                树的简写哈希值                |
| %P   |              父提交的完整哈希值              |
| %p   |              父提交的简写哈希值              |
| %an  |                  作者的名字                  |
| %ae  |                作者的电子邮件                |
| %ad  | 作者修订日期（可以用--date=选项 来定制格式） |
| %ar  |      作者修订日期，按多久以前的方式显示      |
| %cn  |                 提交者的名字                 |
| %ce  |             提交者的电子邮件地址             |
| %cd  |                   提交日期                   |
| %cr  |            提交日期(距今多长时间)            |
| %s   |                   提交说明                   |

> 作者 和 提交者 之间究竟有何差别， 其实作者指的是实际作出修改的人，提交者指的是最后将此工 作成果提交到仓库的人。 所以，当你为某个项目发布补丁，然后某个核心成员将你的补丁并入项目时，你就是 作者，而那个核心成员就是提交者。

当 oneline 或 format 与另一个 log 选项 --graph 结合使用时尤其有用，比如：

```shell
git log --pretty=format:"%h %s" --graph
git log --pretty=oneline --graph
```

#### git log 常用选项

| 选项        |                    说明                    |
| ----------- | :----------------------------------------: |
| -p          |      按补丁格式显示每个提交引入的差异      |
| --stat      |      显示每次提交的文件修改统计信息。      |
| --shortstat | 只显示 --stat 中最后的行数修改添加移除统计 |
| --name-only |    仅在提交信息后显示已修改的文件清单。    |
|             |                                            |
|             |                                            |
|             |                                            |
|             |                                            |
|             |                                            |
|             |                                            |
|             |                                            |

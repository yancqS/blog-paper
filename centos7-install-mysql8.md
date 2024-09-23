---
title: Centos 7.6 安装 mysql 8 小记
description: Centos 7.6 安装 mysql 8 小记
date: '2023-10-04'
cover: 'http://img.up-4ever.site/infinity-4164366.jpg'
top_img: 'http://img.up-4ever.site/infinity-1377518.jpg'
keywords:
  - centos
  - mysql
tags:
  - Linux
  - MySQL
categories:
  - Linux
abbrlink: 91c8425a
---
# Centos 7.6 安装 mysql 8 小记

## 背景

国庆期间，由于发现之前的评论系统好像被墙了，然后萌生了换一个评论系统的想法（不涉及数据迁移，因为根本就没有数据：））。经过比较，最终的方案是[独立部署 waline](https://waline.js.org/guide/deploy/vps.html)到自己的腾讯云服务器。因此需要在服务器上装一个 MySQL 数据库，发现原文作者安装数据库的步骤写的甚是详细，因此转载到博客~~

> 本文转载于[Linux：CentOS7安装MySQL8（详）](https://www.cnblogs.com/secretmrj/p/15600144.html)，对作者表示感谢。正文开始

---

> 基于 Linux 的 `CentOS 7` 安装 `MySQL 8`，包含以下步骤

1. **安装工作**：卸载 MariaDB，下载资源包，检查旧版本 MySQL，解压安装。
2. **配置工作**：基本设置，创建用户组和用户，数据目录。
3. **初始化 MySQL**：配置文件，初始化。
4. **启动 MySQL**：启动服务，登录，修改密码，创建远程连接用户，退出和关闭。

## 安装工作

### 卸载 MariaDB

> MariaDB 是 MySQL 的一个分支，主要由开源社区维护。

1. `CentOS 7+` 不再默认使用 MySQL 数据库，而是 MariaDB 数据库。
2. 直接安装 MySQL 会与 MariaDB 的文件冲突。

#### 卸载

1. **查看版本**：有则需要卸载。

   ```sh
   rpm -qa | grep mariadb
   ```

2. **卸载**：复制文件名，执行以下指令。

   ```sh
   rpm -e --nodeps 文件名
   ```

3. **确认卸载**：

   ```SH
   rpm -qa | grep mariadb
   ```

![](http://img.up-4ever.site/20231004215747.png)

### 下载资源包

> **Hint**：将 MySQL 资源包放在 `/usr/local/` 下

2 种下载方式

#### 方式一：[官网下载](https://dev.mysql.com/downloads/mysql/)

1. **官网下载**：`Linux - Generic (glibc 2.12) (x86, 64-bit)`
2. **上传到服务器**：`/usr/local/` 目录下。

![](http://img.up-4ever.site/20231004220200.png)

#### 方式二：wget 下载

1. **wget 指令**：在 `/usr/local/` 下执行，下载资源包。

   ```SH
   # 格式：wget 资源地址
   wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.20-linux-glibc2.12-x86_64.tar.xz
   ```

2. ![](http://img.up-4ever.site/20231004220238.png)

### 检查旧版本 MySQL

> **指令**：
>
> ```sh
> rpm -qa | grep mysql
> ```

- **没有输出**：说明当前系统没有安装 MySQL，开始安装。

- **有输出**：显示的 MySQL 不是想要的版本，则卸载。

  ```SH
  # 停止服务
  systemctl stop mysqld
  
  # 卸载
  rpm -e --nodeps mysql文件名
  ```

### 解压安装

1. **解压**：tar 指令。

   ```sh
   # .tar.gz 后缀
   tar -zxvf 文件名
   
   # .tar.xz 后缀
   tar -Jxvf 文件名
   ```

2. **解压完成**：`/usr/local/` 下会生成 MySQL 文件夹。

   ![](http://img.up-4ever.site/20231004220555.png)

> **Hint**：将 MySQL 安装在 `/usr/local/` 下
>
> 若没有安装在该目录下，通过 `mv` 指令移动到该目录。

## 配置工作

### 基本设置

#### 文件夹重命名

- 重命名 **MySQL 文件夹**（或**创建软链接** 👉[文件管理 2.5](https://www.cnblogs.com/secretmrj/p/15550426.html)）

- 通常命名为 `mysql版本号`

  ```sh
  # 重命名（也可通过Xftp修改）
  mv 原文件夹名 mysql8
  
  # 软链接
  ln -s 文件夹名 mysql8
  ```

### PATH 环境变量

1. **临时生效**：export 命令（连接会话关闭后失效，通常用于测试环境）

   ```SH
   export PATH=$PATH:/usr/local/mysql8/bin
   ```

2. **永久生效**：修改配置文件。

   ```sh
   vim ~/.bashrc
   # 然后修改 PATH 变量，保存退出
   source ~/.bashrc
   ```

### 确认安装

#### 查看版本

![](http://img.up-4ever.site/20231004221102.png)

确定 MySQL 安装成功后，可删除压缩包。

### 创建用户组、用户

> 注：需要进入 `/usr/local`

1. **创建用户组**：`groupadd`

2. **创建用户**：`useradd`（`-r` 创建系统用户，`-g` 指定用户组）

   ```SH
   groupadd mysql
   useradd -r -g mysql mysql
   ```

### 数据目录

1. **创建目录**：

   ```shell
   mkdir -p /data/mysql8_data
   ```

2. **赋予权限**：

   ```shell
   # 更改属主和数组
   chown -R mysql:mysql /data/mysql8_data
   
   # 更改模式
   chmod -R 750 /data/mysql8_data
   ```

## 初始化 & 启动

### 配置文件

> 在 `/usr/local/etc/` 下创建 `my.cnf` 配置文件，用于初始化 MySQL 数据库

配置内容：

```
[mysql]
# 默认字符集
default-character-set=utf8mb4
[client]
port       = 3306
socket     = /tmp/mysql.sock
[mysqld]
port       = 3306
server-id  = 3306
user       = mysql
socket     = /tmp/mysql.sock
# 安装目录
basedir    = /usr/local/mysql8
# 数据存放目录
datadir    = /data/mysql8_data/mysql
log-bin    = /data/mysql8_data/mysql/mysql-bin
innodb_data_home_dir      =/data/mysql8_data/mysql
innodb_log_group_home_dir =/data/mysql8_data/mysql
# 日志及进程数据的存放目录
log-error =/data/mysql8_data/mysql/mysql.log
pid-file  =/data/mysql8_data/mysql/mysql.pid
# 服务端字符集
character-set-server=utf8mb4
lower_case_table_names=1
autocommit =1
##### 以上涉及文件夹明，注意修改
skip-external-locking
key_buffer_size = 256M
max_allowed_packet = 1M
table_open_cache = 1024
sort_buffer_size = 4M
net_buffer_length = 8K
read_buffer_size = 4M
read_rnd_buffer_size = 512K
myisam_sort_buffer_size = 64M
thread_cache_size = 128
#query_cache_size = 128M
tmp_table_size = 128M
explicit_defaults_for_timestamp = true
max_connections = 500
max_connect_errors = 100
open_files_limit = 65535
binlog_format=mixed
binlog_expire_logs_seconds =864000
# 创建表时使用的默认存储引擎
default_storage_engine = InnoDB
innodb_data_file_path = ibdata1:10M:autoextend
innodb_buffer_pool_size = 1024M
innodb_log_file_size = 256M
innodb_log_buffer_size = 8M
innodb_flush_log_at_trx_commit = 1
innodb_lock_wait_timeout = 50
transaction-isolation=READ-COMMITTED
[mysqldump]
quick
max_allowed_packet = 16M
[myisamchk]
key_buffer_size = 256M
sort_buffer_size = 4M
read_buffer = 2M
write_buffer = 2M
[mysqlhotcopy]
interactive-timeout
```

### 初始化

> **Hint**：需要进入 `/usr/local/mysql/bin`，若添加了 PATH 变量可忽略。

**初始化命令**：注意文件夹名称。

- `--defaults-file`：指定配置文件（要放在--initialize 前面）
- `--user`： 指定用户
- `--basedir`：指定安装目录
- `--datadir`：指定初始化数据目录
- `--intialize-insecure`：初始化无密码（否则生成随机密码）

```sh
mysqld --defaults-file=/usr/local/etc/my.cnf --basedir=/usr/local/mysql8 --datadir=/data/mysql8_data/mysql --user=mysql --initialize-insecure
```

### 启动 MySQL

> 查看 MySQL 的 `/bin` 下是否包含 `mysqld_safe`, 用于后台安全启动 MySQL

![](http://img.up-4ever.site/20231004222042.png)

#### 启动服务

**安全后台启动 MySQL**

```SH
# 完整命令
/usr/local/mysql8/bin/mysqld_safe --defaults-file=/usr/local/etc/my.cnf &
# 若添加了PATH变量，可省略如下
mysqld_safe --defaults-file=/usr/local/etc/my.cnf &
```

![](http://img.up-4ever.site/20231004222147.png)

**确认启动**：第二条即 MySQL 服务。

```SH
ps -ef | grep mysql
```

![](http://img.up-4ever.site/20231004222241.png)

#### 登录

- **无密码**：若以 `--initialize-insecure` 初始化，首次登录时跳过密码。

```SH
mysql -u root --skip-password
```

- **有密码**：若初始化时设置了随机密码，在 `/data/mysql8_data/mysql/mysql.log` 查看

```sh
mysql -u root -p
```

登录后进入 MySQL 命令行：

![](http://img.up-4ever.site/20231004222423.png)

**以下报错，说明 MySQL 服务没开启**

`ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)`

### 修改密码

#### 首次修改

> MySQL **初始化的 root 用户、新创建的用户**，都需要设置首次密码。
>
> 建议使用本地密码插件 `mysql_native_password`。

```sql
# 修改密码
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码';

# 刷新权限
FLUSH PRIVILEGES;
```

#### 日常修改

> 2 种方式

1. **Linux 命令行**

```bash
mysqladmin -u 用户名 -p 旧密码 password 新密码
```

2. **MySQL 命令行**

```sql
# 设置密码
SET PASSWORD FOR '用户名'@'主机' = PASSWORD(‘密码');

# 刷新权限
FLUSH PRIVILEGES;
```

## 远程连接 MySQL

### 创建远程链接用户

1. **选择 mysql 数据库，查看当前用户**

   ```sql
   USE mysql;
   SELECT user,host,plugin,authentication_string FROM user;
   ```

2. `host 字段` 表示可访问当前数据库的主机，目前仅本地可访问。

![](http://img.up-4ever.site/20231004223605.png)

3. 创建用户，任意远程访问

```SQL
# 创建用户
CREATE user 'root'@'%';

# 设置首次密码
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '密码';

# 授权用户所有权限，刷新权限
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%';
FLUSH PRIVILEGES;
```

4. **查看用户**：已创建一个可被任意远程主机访问的 root 用户。

![](http://img.up-4ever.site/20231004224223.png)

### 远程链接 MySQL

> 非原文：腾讯云服务器要开放 3306 端口

### Navicate 远程链接

![](http://img.up-4ever.site/20231004224501.jpg)

> 结束~
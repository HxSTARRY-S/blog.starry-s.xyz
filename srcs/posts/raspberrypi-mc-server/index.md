---
title: 树莓派之我的世界服务器
date: 2016-11-25 15:01:28
layout: post
comment: true
tags:
- Minecraft
- 树莓派
categories:
- 教程
- Minecraft
---
树莓派功能强大，可以像电脑一样干很多事情。刚买树莓派3B时就想弄一个我的世界服务器，但是那时不懂linux，服务器也就没建成。这一阵子打算把这个计划再捡起来，和同学一起联机的话树莓派还是能做到的。ARM系列的CPU性能肯定比X86_64电脑的CPU弱许多，外加上树莓派1G的运行内存，让它跑大型服务器肯定是不可能的，所以只能弄一个几个人联机的小服务器了。

<!--more-->

> 终于可以继续更新博客了！原计划是11月之前更新博客的。不巧电脑出了问题，重装了一遍Arch linux,后来又发生了一些事一直没能用电脑。由于开学到现在能放假供我写博客的时间少之又少，这篇原本在11月初就能发表的文章就这样被硬生生推迟到了11月末了。

# 材料以及工具

* 树莓派一只 （包括所需要的C10的内存卡以及刷好的固件以及5v，2.5A的充电器）
* 一个支持DDNS的路由器（非必须）

> 如果不想用树莓派建服务器的话用按照本教程在电脑上搭建服务器也可以。

# 配置树莓派

**如果你对外开放ssh端口的话一定要设置一个安全的密码**

### 调整GPU可使用的内存：

毕竟用树莓派开服务器不需要占用GPU，直接调到最低即可。

Raspbian系统直接在`raspi-config`中将GPU内存分为16M。
ArchLinux ARM系统在`/boot/config.txt`中更改GPU显存为16即可。

# 安装JAVA

Archlinux ARM:
```
$ sudo pacman -S jre8-openjdk
```

通用方法：
前往 **[JAVA下载页](http://www.oracle.com/technetwork/cn/java/javase/downloads/jdk8-downloads-2133151-zhs.html)** 下载树莓派（ARM架构，32位系统）所支持的JDK。

```
$ sudo mkdir ./java
$ sudo tar -zxvf 你所下载的JDK.jar.gz -C ./java/
```
在终端输入`./java/jdk1.8.0/bin/java -version`显示java的版本号。

# 部署服务器

本篇使用[Paper](https://github.com/PaperMC/Paper)部署服务器。
当然除了Paper之外还有Bukkit和Spigot可选。

使用以下命令启动服务器。

```
$ echo eula=True > eula.txt
$ ./java/jdk*/bin/java -Xms512M -Xmx1024M -jar ./spigot.jar
```
第一次运行会下载一些文件需要一定的时间。

网上有很多服务器插件和Mod什么的。本章中不做过多介绍.

# 用screen 保持服务器一直运行而不被关掉。

首先安装好screen。
```
$ screen
$ ./java/jdk1.8.0/bin/java -Xms512M -Xmx1024M -jar./paperclip.jar
```
在配置好你的服务器后就可以和小伙伴一起联机了。经过小伙伴测试几个人简单的联机运行蛮正常的。

# 使用动态DNS帮助你和外网的小伙伴一起联机MC～

**想使用动态域名解析服务就必须保证这样几个条件:**

1. 路由器拨号后运营商给你分配了一个公网ip而不是局域网（内网）ip。
2. 运营商公司至少给你开放了一个端口。

通常情况下以上两个问题都是~~不容易遇到的~~，不过一旦点子不好的话，就只能靠别的方法把你内网的数据影射到外网了（例如买个棒子什么的）。

如果运营商给你分配了一个内网IP，请自行打电话与运营商联系。

## 配置路由器DDNS

进入路由器的动态DNS配置界面：

不同的路由器配置方法不同，请自行摸索。

重启路由器后生效。可以ping一下你的域名看看能不能ping到你的ip地址。。

## 端口映射
弄好动态域名解析服务后就需要设置好端口映射，电脑版MC端口为25565，手机版MC端口为19132，设置好端口映射后进入我的世界添加服务器输入域名就可以联机了！

<br/>

# Happy MC!
<br/>
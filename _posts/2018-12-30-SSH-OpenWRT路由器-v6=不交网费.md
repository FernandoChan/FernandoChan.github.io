---
layout: post
title: "使用OpenWrt路由器连接IPv6"
subtitle: "SSH+OpenWRT路由器+IPv6=不交网费?"
author: "Fernando"
header-img: "img/post-bg-universe.jpg"
header-mask: 0.2
tags:
  - OpenWrt
  - SSH
  - IPv6
  - 计算机网络
  - 搞机
typora-copy-images-to: ..\img
---





# 背景

最近在学了计算机网络之后, 一直有点心痒痒. 我们学校的教育网一直都支持IPv6, 可惜自己交了两年的网费, 却没有获得过其中的便利, 尤其是连上了路由器以后,一直都在用自己买的服务器科学上网, 踩过的坑不计其数.

加上这两周下载游戏的时候, 实在难以忍受区区几百KB/s 的垃圾网速. 

所以决定在自己的路由器上好好折腾一番

# 目标

晚上不断网

不交网费

科学上网

实现IPv6

更进一步在宿舍的WiFi上实现整个宿舍所有设备的v6访问(不知可不可行)

# 准备

- 一个海外服务器(Vultr)
- SSH软件Putty
- 一个已经刷了OpenWrt操作系统的路由器(两年前已经配置好了, 当时懵懵懂懂照着`华工路由器群`一步步来, 如今居然已经可以看懂大部分内容了. 同学, 华工路由器群了解一下)

# 参考资料

#### [基于 OpenWrt 路由器和 SSH 的终极翻墙解决方案](https://linuxtoy.org/archives/openwrt-and-ssh.html)

主要参考的是前面 ssh 登录路由器, 并且使用ssh公钥-密钥对使得自己在本地PC以及路由器上登录远程服务器都不需输入密码, 使用autossh 使其每次有访问请求的时候自动登录远程服务器

主要的原理就是

1. `ssh-keygen -t rsa` 在本地PC的`c:\Users\Administrator\` 下生成  一对`RSA` 加密算法的公钥`.ssh\id_rsa.pub` 和密钥`.ssh\id_rsa` 对. 
2. 把公钥通过`scp id_rsa.pub root@45.77.12.24:/root` 复制到远程服务器并且改名`cat ~/id_ras.pub >>/root/.ssh/authorized_keys` 加入到已验证的keys里面.
3. 把密钥通过`scp id_rsa root@192.168.1.1:/root`  复制到路由器并且放入`.ssh` 目录下.
4. 这时候就可以通过`ssh root@45.77.12.24` 无需密码地访问到远程主机, 既可以在PC上, 也可以在路由器上访问了.

#### 远程登录的更详细资料可以看这个文章 [ssh 公钥登陆，清除公钥，批量分发脚本，首次远程登录不用输入yes](https://blog.csdn.net/fanren224/article/details/63250184)

虽然说的是两台主机, 但是只要把vultr远程服务器和路由器的系统都看做一台主机就可以了, 原理是一模一样的

#### [OpenWrt命令行](https://openwrt.org/zh-cn/doc/howto/user.beginner.cli)

#### [教育网DD-WRT／OpenWrt用上IPv6——以南京大学为例](https://www.polarxiong.com/archives/%E6%95%99%E8%82%B2%E7%BD%91DD-WRT-OpenWrt%E7%94%A8%E4%B8%8AIPv6-%E4%BB%A5%E5%8D%97%E4%BA%AC%E5%A4%A7%E5%AD%A6%E4%B8%BA%E4%BE%8B.html)

下一个问题就是怎么在路由器访问v6

#### [OpenWRT IPv6 三种配置方式OpenWRT IPv6 三种配置方式](http://blog.kompaz.win/2017/02/22/OpenWRT%20IPv6%20%E9%85%8D%E7%BD%AE/)

- IPv6 中继
- IPv6 穿透
- IPv6 NAT

主要讲的是以上的三种方式, 以及各自的对比

# 步骤



```powershell
在管理界面--网络--接口中查出路由器WAN口的IPV6全域地址 >
IPv6: 2001:250:3000:3CC3:82FA:5BFF:FE31:7E2E/64

用网线直连电脑的:
2001:250:3000:3cc3:b802:e7f:aa91:cb7
用过CMD tracert -6 2404:6800:4005:80a::200e 获得的IPv6网关: 
2001:250:3000:3cc3::1
```
![1546178597268](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1546178597268.png)
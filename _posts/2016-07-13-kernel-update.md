---
title: centos2.6.18升级到2.6.39
author: k
layout: post
permalink: /c-face-no1/
post_views_count:
  - 0
categories:
  - C/c++
  - 引言
tags:
  - 内核升级
---


本来想升级到最新版本4.6的，但是不成功，即使3.4也不成功，暂且先升级到2.x最新版本。

步骤如下：

一 下载源码

	https://www.kernel.org/pub/linux/kernel/

解压到/usr/src目录下

二 更新

	#yum groupinstall “Development Tools”

	#yum install ncurses ncurses-devel qt-devel hmaccalc zlib-devel binutils-devel elfutils-libelf-devel

	#yum update && yum upgrade

三 编译安装

	#cd linux-2.6.39

	#cp /boot/config-`uname -r` .config

	#makemenuconfig


一定要勾选（输入y选中，M编译为模块）

	a、General setup→[*] enable deprecated sysfs features to support old userspace tools
	b、Processor type and features→HighMemory Support。
	c、找到以下选中选项并选中：
	networking support → networking options → network packet filtering framework(netfilter)
	(1)Corenetfilter configuration
	. 勾中"Netfilter connection tracking support" -m state相关模块是依赖它的，不选则没有。
	. 将netbios name service protocal support(new) 编译成模块,不然后面升级iptables后启动时会出错
	. 勾中"Netfilter Xtables support (required for ip_tables)"

	(2)IP: Netfilter Configuration
	. 将 "IPv4 connection tracking support (require for NAT)" 编译成模块。
	. 勾中IP tables support (required for filtering/masq/NAT) 。
	. 将 "Full NAT" 下的 "MASQUERADE target support" 和 "REDIRECT target support" 编译成模块


	#make -j4 bzImage  //生成内核文件

	#make -j4 modules  //编译模块

	#make -j4 modules_install  //编译安装模块

	#make install

	#make clean && make mrproper #完成或者安装过程出错，可以清理上次编译的现场

四 设置启动

安装完成后，需要修改Grub引导顺序，让新安装的内核作为默认内核。
编辑 grub.conf文件，

	#vi /etc/grub.conf

	#boot=/dev/sda

	default=0

	timeout=5

	...

数一下刚刚新安装的内核在哪个位置，从0开始，然后设置default为那个数字，一般新安装的内核在第一个位置，所以设置default=0。
	#reboot

	#uname -r

	2.6.39

至此，成功升级内核

 

有时候报错：


修改.config文件中CONFIG_SYSFS_DEPRECATED_V2，将原本被注释掉的

       CONFIG_SYSFS_DEPRECATED_V2 改成CONFIG_SYSFS_DEPRECATED_V2=y

重新编译安装
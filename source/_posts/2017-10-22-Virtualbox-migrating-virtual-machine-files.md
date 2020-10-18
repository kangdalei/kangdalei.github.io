---
title: Virtualbox_migrating_virtual_machine_files
date: 2017-10-22 16:27:39
updated: 2020-10-18 16:27:39
tags:
categories:
description:
---
本质上搞定
[UUID](<https://en.wikipedia.org/wiki/Universally_unique_identifier>)
。VirtualBox新建虚拟磁盘时会给它分配一个独一无二的UUID，
以后迁移时需要为这个虚拟磁盘文件新建一个UUID。

<!--more-->

1. 本机内迁移
=============

一般用于更改默认安装路径， 把以前安装的虚拟机存储文件迁移到新目录。

使用 \`VBoxManage.exe clonehd\` 命令。

原虚拟磁盘位置： "C:\Users\lei\VirtualBoxVMs\debian9-64\debian.vmdk"

迁移的目标地址："D:\virtualbox\debian"

假设 VirtualBox 安装在 "C:\ProgramFiles\Oracle\VirtualBox"， 此目录下有
VBoxManage.exe 这个文件。

在该目录运行命令行 ( 在文件夹空白地方按Shift键和鼠标右键, 有快捷菜单 )

``` {.bash}
C:\Program Files\Oracle\VirtualBox>VBoxManage.exe clonehd "C:\Users\lei\VirtualBox VMs\debian9-64\debian.vmdk" "D:\virtualbox\debian\debian.vmdk"
```

OK， 会有进度提示。然后新建虚拟机时选择已有虚拟磁盘即可。

2. 不同机器上复制
=================

使用 \`VBoxManage.exe internalcommands sethduuid\` 命令.

从其他机器上复制过来的虚拟磁盘文件. 假设放在了
"D:\virtualbox\debian\debian.vmdk"

``` {.bash}
C:\Program Files\Oracle\VirtualBox>VBoxManage.exe internalcommands sethduuid "D:\virtualbox\debian\debian.vmdk"
```

---
title: Linux命令行下的退格键
date: 2017-10-10 15:19:49
updated: 2020-10-18 15:19:49
tags:
    - linux
categories:
description:
---

起源
====

有时候为了学好Linux, 得要懂一些Unix考古学.
比如为什么Emacs的默认快捷键那么难按, 又比如烦人的换行符 `\r`和`\n`,
以及这次的`^H` `^?` `^[[3~` (`ESC[3~`).

<!--more-->

目前我看到讲考古学讲得最好的书是《UNIX&LINUX大学教程》("Harley Hahn's
Guide to Unix and Linux", 作者:Harley Hahn). 书中第七章 Unix键盘使用,
讲述了`^H ` `^?` 的来龙去脉, 但是没有包含 `^[[3~`.

[wikipedia](<https://en.wikipedia.org/wiki/ASCII>)
上列举了各ASCII码的二进制\十六进制\终端输入\C语言表示方法.

简单来说,

<table border="1">
<tr>
<td>终端</td>
<td>名称</td>
<td>ASCII (16进制)</td>
<td>备注</td>
</tr>
<tr>
<td>^H</td>
<td>BS 退格</td>
<td>0X08</td>
<td>键盘上CTRL+H得到</td>
</tr>
<tr>
<td>^?</td>
<td>Del</td>
<td>0X7F</td>
<td>只是一种表示方法,按CTRL+? 键得不到这个键值.<br> 有时可以通过PC机键盘按 CTRL+Backspace键得到这个值 </td>
</tr>
<tr>
<td> ^[[3~</td>
<td>Delete</td>
<td>0X1b, 0X5b, 0X33, 0X7e 四字节 </td>
<td> VT220时代, PC机键盘上的Delete键   </td>
</tr>
</table>

[参考skywind: Vim 里如何映射 CTRL-h 为 left ?](http://www.skywind.me/blog/archives/1857)

Linux上erase信号表示删除最后键入的字符. 这个信号即可以绑定到终端的`^H`上,
也可以绑定到终端的`^?`上. (甚至随便你喜欢的哪个键. )
至于具体绑定在终端哪个键值上,终端上输入 `stty -a` 可以看到.

显示按键的ASCII码命令 showkey -a
================================

在终端里面输入 showkey -a 然后输入按键,
可以得到这个按键的ASCII码和二进制,十六进制表示.输入CTRL+D 结束.

可以自行实验一下自己键盘上的Backspace、CTRL+Backspace、CTRL+H、Delete
都是什么值.

使用远程登录工具的一个常见问题就是远程系统上erase信号和本地键盘Backspace键不匹配,
本来是想删除前一个字符, 屏幕上却输入了^H.

这篇文章 ( <https://www.zhihu.com/question/23550774/answer/132576876> )
讲述了各终端模拟器修改Backspace键的键码方法.

更改一下终端的erase信号绑定值也是一种方法: `stty erase ^H` 或者 `stty erase ^?`

Emacs的C-h和^H
===============

远程登录工具MobaXterm 9.1 默认也是把erase信号绑定到了`^H`,
Backspace默认也发出`^H`键. CTRL+H键也发送`^H`, 可以删除前面的字符.

但是如果在终端里使用Emacs, 就会发现它的帮助前缀键 C-h 和 `^H` 冲突,
按CTRL+H 是删除字符. 这个时候只能按\<F1\>键来代替C-h

解决办法或者在Emacs的配置文件里重新键绑定, 或者stty erase `^?`
然后把Backspace键改为 `^?` ( MobaXterm里就是取消勾选Backspace的`^H`设置,
Backspace键就会变为`^?`. )

Xshell 5下的特点
================

**Xshell刚爆出有后门漏洞, 需要升级到最新版.**

Xshell 5默认erase绑定到 `^?`, 但是它有个有趣的设置.
如果键盘功能键类型设置为默认状态, Backspace键序列选`^?`, 或者`^H`,
都可以起删除前面字符的作用. CRTL+H键也能删除字符. 看起来它是自动调整了.
而且在它里面使用Emacs, CRTL+H键又恢复成了帮助前缀键, 而不是删除键.

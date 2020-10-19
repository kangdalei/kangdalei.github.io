---
title: Emacs & keyboard 按键二三事
date: 2017-10-13 13:38:04
updated: 2020-10-17 13:38:04
tags:
    - emacs
categories:
description:
---
Emacs的按键是出了名的难按. 以至于新手入门第一件事就是改键盘布局,
否则没法学下去. 参考: Effective Emacs（中文）

难道不是很奇怪吗? 为何"神的编辑器"如此古怪难用? 其实这主要是因为Emacs远远早于
现代PC机键盘而出现,它的快捷键不是在PC机键盘上设计的, 而是在早已消失的Lisp机键
盘上敲定的. (参见: Peter Seibel 《编程人生》 Guy Steele 访谈篇)

![](/images/Space-cadet_keyboard.jpg)

Space-Cadet~keyboard~-2m.jpg

<https://en.wikipedia.org/wiki/Space-cadet_keyboard>
<!--more-->
## 历史 ##

李杀写过一篇 [Why Emacs's Keyboard Shortcuts are
Painful](http://ergoemacs.org/emacs/emacs_kb_shortcuts_pain.html).
可以看到当时lisp机键盘上, Ctrl键在空格键两旁,
用强壮的大拇指往回轻轻一勾, 即可. 原则是, 如果要按左边的键, 比如a,
就按右边Ctrl键, 双手配合. 反之亦然.

## Meta键 ##

wikipedia : [Meta key](https://en.wikipedia.org/wiki/Meta_key)

Meta键也是以前Lisp机键盘上的修饰键, 现在只有用其他键来仿真.
比如PC机上的:

-   Alt键 仿真Meta键, 一般都能很好工作. 比如, M-x 就是按住Alt键,
    同时按住x键.
-   ESC键 按下ESC键, 松开, 表示按了meta键, 然后再按其他键.
-   Ctrl-[ 可以来代替 Esc 键。如果远程运行Emacs，而终端又无法使用 Esc 和
    Alt 键，这种方法可以解燃眉之急。因为 \^[
    就是在终端输入ESC键值(0x1B)方法. 在我的MobaXterm~Personal上~, Ctrl-3
    也可以输入ESC键值(0x1B).

大家可以在终端用命令

``` {.bash}
 showkey -a
```

来查看各种修饰键搭配按键的输入值.

## 标记按键 ##

``` {.bash}
 M-x set-mark-command
```

标记位置命令是一个非常频繁使用的命令.
Emacs默认把这个命令绑定到了Ctrl-SPC和Ctrl-@. 但是对中文用户来说,
Ctrl-SPC一般已被输入法占用, 而Ctrl-@又太难按,
需要三根手指极其别扭的姿势去按.

常见做法是把这个命令绑定到其他按键, 比如:

(global-set-key (kbd "C-;") 'set-mark-command)

但是设计者为何要把高频命令分配到 C-@ 呢? 因为在终端上, C-@ 只需按 Ctrl-2
即可, 不需要同时按Shift键. 只是如今的GUI界面, 系统区分了C-@和C-2,
才出现了难按的问题.

而且, 在终端上C-; 这种按键方式是被忽略的, 系统不予相应.
所以之前的键绑定命令只能工作在GUI版本上.

## 参数命令 C-u ##

> C-u
> 是一个通用参数命令，后面接一个数字和一个命令。它将按某个次数多次运行特定的命令。

其实在GUI版本里, 默认情况下, 你直接按Ctrl键或者Alt键, 同时按一个数字,
也可以达到C-u的效果.

比如删除到行首的命令 \`C-u 0 C-k\` 比较啰嗦. 其实按 \`C-0 C-k\` 即可.
实际按键时, 按住Ctrl键不放, 分别按下0和k.

但是, 在终端下(不是虚拟终端), 这种按法就失灵了.
因为系统把修饰键+数字键解释为别的键值.

## M-w 和QQ的快捷键 ##

如果运行Emacs的时候同时开着QQ, 会发现Alt-w键失灵.
因为Alt-w也是QQ的默认快捷键, 被QQ提前拦截了. 可以在QQ上取消这个快捷键.
腾讯新推出的TIM上也有这个默认快捷键, 但是目前却没有取消的方法.

## \<F1\> C-h 和 Del ##

如果在终端上发现C-h失灵, 多半是因为C-h被绑定到了主机的erase信号上,
变成了del键. 可以用\<F1\>键代替.

## 跳转匹配括号的 C-M-b 和 C-M-f ##

这种组合按键, Ctrl-Alt-f 可以工作, Ctrl-Alt-b 却失灵. 这时可以先按
ESC键, 松开, 然后再按C-b.

## 我的按键方式 ##

我个人把空格右边的Alt键和Ctrl键互换位置. 按Ctrl键时, 右手大拇指回勾.
按Alt键时, 左手大拇指回勾. 再装上smex插件. 常用命令用键绑定,
不常用的命令直接M-x, 输入命令的头几个字母后, smex插件会智能提示命令名,
如果第一个就是你想输入的命令, 直接回车, 不用把命令敲全.

网上常见把左下角Ctrl键和Caps Lock键互换位置, 我并不看好.
因为Ctrl键是组合键, C-a不是一样别扭吗?
纤细的小拇指不适合这种频繁的组合按键工作. Vim党把Esc键放到Caps
Lock键那里没问题, 只单独按一次抬手即可, 不牵扯组合按键.

还有一种用手掌去压左下角和右下角Ctrl键的招法.
可是如果碰到笔记本那种按键是平的键盘, 就失灵了.

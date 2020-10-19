---
title: Emacs 字体设置
date: 2020-10-19 08:55:16
updated: 2020-10-19 08:55:16
tags:
    - emacs
categories:
description:
---
Emacs 的字体设置比较繁杂, 还能对不同的字符集设置不同的字体, 一些技巧记录于此.

<!--more-->

## 调出字体设置菜单 ##

1. Shift+鼠标左键, 对当前buffer的字体, 改变、增大、减小、恢复默认.

> \<S-down-mouse-1\> (translated from \<S-mouse-1\>) at that spot runs the command
> mouse-appearance-menu (found in global-map), which is an interactive
> compiled Lisp function in ‘mouse.el’.
>
> It is bound to \<S-down-mouse-1\>.
>
> (mouse-appearance-menu EVENT)
>
> Show a menu for changing the default face in the current buffer.

2.  菜单 "Options-\> Set Default Font"

## 中英文只设置一种字体 ##


把光标放到你所在的字体上，命令 `M-x describe-font` 来查看你当前使用的字体名称、
字号大小.  显示的信息是相对古老的描述方式, 字号也采用emacs自己的大小

把字体信息拷贝, 写入如下代码:

``` emacs-lisp
(set-default-font "-outline-Yahei Mono-normal-normal-normal-mono-20-*-*-*-c-*-iso8859-1")
```

实际上 `set-default-font` 函数在 emacs 23 已经过时, 使用 `set-frame-font` 代替,
字体信息也可以使用更现代的方式:

```
(set-frame-font "Yahei Mono-12")
```

"Yahei Mono"是在菜单 "Options-\> Set Default Font" 看到的字体名称, "-12" 是字号大小.

使用这种方法设置字体, 在等高或者等宽方面, 总不舒服, 而且在`C-x C-=` 或者 `C-x
C--` 增大减小字体时, 只有英文变化.

## 进阶设置 ##

参考文章:
- [emacs 字体选择机制](https://www.cnblogs.com/eadle/p/10658661.html)
- [狠狠折弯了一把emacs中文字体](http://baohaojun.github.io/perfect-emacs-chinese-font.html)
- [emacs中英文等宽字体设置](https://www.cnblogs.com/penggy/p/7475831.html)
- [Emacs 字体与字符集](https://archive.casouri.cat/note/2019/emacs-%E5%AD%97%E4%BD%93%E4%B8%8E%E5%AD%97%E4%BD%93%E9%9B%86/)

[emacs文档: font selection](https://www.gnu.org/software/emacs/manual/html_node/elisp/Font-Selection.html) 解释:
emacs在把一个字符编码绘制到屏幕前, 会根据它的face属性([文档](https://www.gnu.org/software/emacs/manual/html_node/elisp/Displaying-Faces.html) ), 选择字体.

> 无论是Wiki还是能找到的资料，对于Face的定义都是”关于要显示出来的东西的外在属性的定义“，包括font的属性（family，width，slant等等），还有颜色、下划线等等等等（Emacs wiki上甚至说“我们需要一个明确的定义”）。话句话说，face指定了我们会看到什么东西。
>
> 同一节指出了合并face的属性的优先级。其中最低的优先级是default face，也就是我一开始查到的命令所设置的东西，使用 M-h f set-face-attribute可以得到


...待续







end here

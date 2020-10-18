---
title: Emacs auto save file
date: 2017-11-11 14:32:16
updated: 2020-10-17 14:32:16
tags:
categories:
description: Emacs的文件备份机制
---
本文是对这篇文章
[Emacs的临时文件和备份文件](http://blog.useasp.net/archive/2014/07/18/emacs-temporary-files-and-backup-files-for-edited-file.aspx)
的细化和进一步说明.

临时文件(Auto-Save file)
========================

正常情况下emacs退出后会删掉这个文件,
所以我觉得临时文件这个汉语名字更为准确一些. Auto-Save这个名字,
我开始以为是自动保存到原文件去的意思, 比如
ManateeLazyCat在“Emacs：我已经十年没有按过保存按键了”
里的意思。[auto-save.el](https://github.com/manateelazycat/deepin-emacs/blob/master/site-lisp/extensions/lazycat/auto-save.el)

但是用过一段上面的 auto-save.el 后, 我还是更喜欢emacs默认的保存方式.
由自己决定什么时候把缓冲区中的文件保存到原文件中去. 默认情况下,
如果输入超过300个字符或是键鼠无动作超过30秒,
emacs就会自动把内存里的缓冲区变动保存到临时文件 \#file-name\# 中.
如果由于断电等原因emacs异常退出, 没来得及保存, 可以使用 M-x recover-file
来恢复文件.

可以设置自动保存的参数, 比如:

``` {.elisp}
(setq-default auto-save-timeout 15) ; 15秒无动作,自动保存
(setq-default auto-save-interval 100) ; 100个字符间隔, 自动保存
```

备份文件(Backup files)
======================

因为会在原目录中留下 "file-name\~" , 所以大家都比较烦. 而且现在可以通过
**git** 管理版本, 所以一般关掉这个功能.

``` {.elisp}
(setq make-backup-files nil)
```

如果想保留这个功能, 又不想在当前目录下留下一堆 "file-name\~" 烦人,
可以指定集中保存备份文件的目录.

``` {.elisp}
(setq
     backup-by-copying t ; 自动备份
     backup-directory-alist
     '(("." . "~/.em_backup")) ; 自动备份在目录"~/.em_backup"下
     delete-old-versions t ; 自动删除旧的备份文件
     kept-new-versions 3 ; 保留最近的3个备份文件
     kept-old-versions 1 ; 保留最早的1个备份文件
     version-control t) ; 多次备份
```

自动备份文件有两种模式: **改名(Renaming)和复制(Copying)**.
默认是改名模式. 两种模式的区别可见 [18.3.2.3 Copying vs.
Renaming](https://www.gnu.org/software/emacs/manual/html_node/emacs/Backup-Copying.html)
. 只有文件存在硬链接而且有多个文件名的情况下, 复制(Copying)
模式才有意义. 在改名模式下, 其他硬链接会依然链接到备份文件上.
而复制模式下, 其他硬链接会链接到新文件上.

打开复制模式:

``` {.elisp}
(setq backup-by-copying t) ;使用复制模式备份文件
```

参考:

> <https://stackoverflow.com/questions/151945/how-do-i-control-how-emacs-makes-backup-files>
>
> If you've ever been saved by an Emacs backup file, you probably want
> more of them, not less of them. It is annoying that they go in the
> same directory as the file you're editing, but that is easy to change.
> You can make all backup files go into a directory by putting something
> like the following in your .emacs.
>
> (setq backup-directory-alist \`(("." . "\~/.saves")))
>
> There are a number of arcane details associated with how Emacs might
> create your backup files. Should it rename the original and write out
> the edited buffer? What if the original is linked? In general, the
> safest but slowest bet is to always make backups by copying.
>
> (setq backup-by-copying t)
>
> If that's too slow for some reason you might also have a look at
> backup-by-copying-when-linked.
>
> Since your backups are all in their own place now, you might want more
> of them, rather than less of them. Have a look at the Emacs
> documentation for these variables (with C-h v).
>
> (setq delete-old-versions t kept-new-versions 6 kept-old-versions 2
> version-control t)
>
> Finally, if you absolutely must have no backup files:
>
> (setq make-backup-files nil)
>
> It makes me sick to think of it though.

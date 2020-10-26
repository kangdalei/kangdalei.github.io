---
title: Windows 系统下 Gvim 安装配置
date: 2020-10-26 22:34:58
updated: 2020-10-26 22:34:58
tags:
- vim
categories:
description:
---

记得第一次学vim是2008年, 跟着 vimtutor 以及 [善用佳软](http://xbeta.info/) 的新
浪博客学的.  后来又学了emacs, 就把vim放弃了.  主要是因为vim输入中文问题.  现在的
主力编辑器还是 emacs + evil.

本文记录安装以及配置过程, 以方便将来在新电脑上迁移.

<!--more-->

## 安装Gvim ##

[官网](https://www.vim.org/) [下载地址](https://www.vim.org/download.php)

有时大陆打开, 会有问题.  这里分享一个坚果云链结 [gvim80-586.exe](https://www.jianguoyun.com/p/DauixVMQtp6cBhjWnMgD)

## 配置文件 ##

windows下配置文件 `_vimrc` 放在 `C:\Users\<usename>` 下

目前的配置如下:

```
"keymap =======================================================

imap jj <esc>

let mapleader = "\<Space>"
"let mapleader = ","
nnoremap <Leader>sv :source ~/_vimrc<CR>
nnoremap <Leader>ev :tabnew ~/_vimrc<CR>


nnoremap <Leader>bt :call libcallnr
            \("c:\\Users\\kdl\\.vim\\plugged\\VimTweak\\vimtweak.dll", "SetAlpha", 180)<CR>
            \:set nonumber<CR><CR>
nnoremap <Leader>br :call libcallnr
            \("c:\\Users\\kdl\\.vim\\plugged\\VimTweak\\vimtweak.dll", "SetAlpha", 255)<CR>
            \:set number<CR>

"General=======================================================
set nocompatible  "去掉讨厌的有关vi一致性模式，避免以前版本的一些bug和局限
syntax on  "语法高亮
set showcmd
set showmode
set hlsearch " 搜索高亮
set incsearch " 增量查找
"set is " 未输入完毕就开始搜索
set showmatch
set backspace=indent,eol,start "退格键正常，去掉与vi的一致性
" backspace，h，左右方向键，跨行作用
set whichwrap=b,h,<,>,[,]
" Vim 的默认寄存器和系统剪贴板共享
set clipboard+=unnamed

set ruler "显示光标行号列号
set autoread "文件改动时，自动载入
set number "显示行号
set wrap "自动折行
set textwidth=0 "启动后默认值78，会自动插入回车
"set tw=80
" 保留历史记录
set history=400

"缩进设置--------------------------------------------
set shiftwidth=4 "自动缩进字符数
set tabstop=4  " 制表符宽度
set expandtab   "自动把tab转成空格，
"如果此时需要输入真正的 tab，则输入 Ctrl+V, tab，在 Windows 下是 Ctrl+Q, tab
set smarttab
set shiftwidth=4  "表示每一级缩进的长度
set softtabstop=4  "编辑模式的时候按退格键的时候退回缩进的长度
filetype indent on "文件类型检测


" 状态栏-------------------------------
set laststatus=2 "显示状态栏
set statusline=[B:%n]\   "缓冲区编号
"set statusline+=%9.20F\   "全路径文件名，显示最短9-20个字符
set statusline+=%t\    "文件名，无路径
set statusline+=%m%r%h%w\
set statusline+=%= "左对齐和右对齐分界
set statusline+=Ln:%l\/%L,\ Col:%v\
set statusline+=[%p%%]\
set statusline+=[%{&ff}:%{&fenc!=''?&fenc:&enc}:%{(&bomb?\",BOM\":\"\")}]\
set statusline+=[%Y]\ "文件类型
"set statusline+=%{strftime(\"%d/%m/%y\ -\ %H:%M\")}\
"set statusline=%1*\%<%.50F\             "显示文件名和文件路径 (%<应该可以去掉)
"set statusline+=%3*\%{&ff}\[%{&fenc}]\ %*   "显示文件编码类型
"set statusline+=%4*\ row:%l/%L,col:%c\ %*   "显示光标所在行和列
"set statusline+=%5*\%3p%%\%*            "显示光标前文本所占总文本的比例
"


"Lang & Encoding=======================================================
"编码设置
set encoding=utf-8
set fileencodings=utf-8,gbk,ucs-bom,cp936
"set fileencoding=utf-8 "强制以utf-8保存文件
"解决菜单乱码
set langmenu=zh_CN.utf-8
source $VIMRUNTIME/delmenu.vim
source $VIMRUNTIME/menu.vim
"解决consle输出乱码
language messages zh_CN.utf-8
set termencoding=gbk

"防止特殊符号无法正常显示。在 Unicode "中，许多来自不同语言的字符，
"如果字型足够近似的话，会把它们放在同一个编码中。但在不同编码中，字
"符的宽度是不一样的。例如中文汉语拼音中的 "ā 就很宽，而欧洲语言中
"同样的字符就很窄。当 VIM 工作在 Unicode "状态时，遇到这些宽度不明
"的字符时，默认使用窄字符，这会导致中文的破折号“——”非常短，五角
"星“★”等符号只能显示一半。因此，我们需要设置 "ambiwidth=double 来
"解决这个问题。
set ambiwidth=double

"GUI=======================================================
set guifont=Yahei_Mono:h12:cANSI
"
"if has("gui_running")
"    "au GUIEnter * simalt ~x  " 窗口启动时自动最大化
"    "set guioptions-=m       " 隐藏菜单栏
"    "set guioptions-=T        " 隐藏工具栏
"    "set guioptions-=L       " 隐藏左侧滚动条
"    "set guioptions-=r       " 隐藏右侧滚动条
"    "set guioptions-=b       " 隐藏底部滚动条
"    "set showtabline=0       " 隐藏Tab栏
"endif

set guioptions-=r
set guioptions-=m
set guioptions-=T
"<F2>切换菜单和工具栏显示隐藏
map <silent> <F2> :if &guioptions =~# 'T' <Bar>
        \set guioptions-=T <Bar>
        \set guioptions-=m <bar>
    \else <Bar>
        \set guioptions+=T <Bar>
        \set guioptions+=m <Bar>
    \endif<CR>

"颜色主题设置
"hi Normal guibg=#99cc99 guifg=Black   "gui颜色设置
"hi LineNr guibg=#003366 guifg=#99ccff ctermbg=7777 ctermfg=blue  "cmd颜色设置
set cursorline "光标所在行高亮
"当前光标行颜色设置
"hi CursorLine cterm=NONE ctermbg=darkred ctermfg=white guibg=#66cc99 guifg=black

set background=light "亮色
"set background=dark
colorscheme solarized
"Format =======================================================


"Plugin =======================================================
" 插件配置

call plug#begin('~/.vim/plugged') "Vim 插件的安装路径，可以自定义。
"~ 表示系统路径，在windows下为 C:/User/username/
":PlugInstall
Plug 'godlygeek/tabular' "必要插件，安装在vim-markdown前面
Plug 'plasticboy/vim-markdown'
"Plug 'mzlogin/vim-markdown-toc'
Plug 'lyokha/vim-xkbswitch' "退出insert模式，自动输入法切换
Plug 'vim-scripts/VimTweak' "windows下gvim背景透明
call plug#end()

let g:XkbSwitchEnabled     = 1
let g:XkbSwitchIMappings   = ['cn']
let g:XkbSwitchIMappingsTr = {'cn': {'<': '', '>': ''}}


"Function =======================================================


```
## 主题配色

vim自带的一些主题.

上述配置文件使用了 ` solarized`
[github](https://github.com/altercation/vim-colors-solarized)

把 solarized.vim 放入gvim安装目录的 colors 文件夹下. 我的目录位置是:


``` powershell
C:\Program Files (x86)\Vim\vim80\colors

```

solarized 的readme中说, 可以把 solarized.vim 放入 `~/.vim/colors/` 中. 我这里报
错, 说找不到这个文件.

其实, 放到 `C:\Users\<username>\vimfiles\colors ` 这个目录也可以. 这个目录应该
是vim安装程序生成的.

## 安装vim-plugin插件管理器 ##

首先, 建立 .vim目录.  vim 的插件集中存放在 ~/.vim 目录中.

windows的资源管理器无法建立 `.`开头的文件夹, 可以在cmd命令行窗口创建

```
mkdir .vim
mkdir .vim\plugged
```

使用 [plug.vim](https://github.com/junegunn/vim-plug) 插件管理器.

安装方法:

Windows (PowerShell)

```
md ~\vimfiles\autoload
$uri = 'https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
(New-Object Net.WebClient).DownloadFile(
  $uri,
  $ExecutionContext.SessionState.Path.GetUnresolvedProviderPathFromPSPath(
    "~\vimfiles\autoload\plug.vim"
  )
)
```

如果出错, 就手工把下载的 plug.vim 插件放入  `~\vimfiles\autoload\` 目录


进入vim后 :PlugInstall
注意大小写！

## 中文输入法自动切换插件配置： ##

参考文章:
1. [解决恼人的vim中文输入法切换视频](https://zhuanlan.zhihu.com/p/49411224)
2. [vim自动切换输入法](https://www.zhihu.com/question/25744174/answer/506519877)


步骤:
1. 安装 lyokha/vim-xkbswitch 插件 <https://github.com/lyokha/vim-xkbswitch>. 配置文件如果生效, 自动下载, 并配置.
2. windows系统安装美式键盘. 设置 --> 时间和语言 --> 语言 --> 添加首选的语言，选择“英语(美国)”
3. 下载 libxkbswitch64.dll <https://github.com/DeXP/xkb-switch-win> , 放入和gvim.exe同目录中。

# Hexo 安装

## 注册github帐号以及建立仓库

点击GitHub中的New repository创建新仓库，仓库名应该为： `用户名.github.io`. 比如我的例
子: `kangdalei.github.io`

注意, 现在github的仓库默认主分支是 main.  需要在仓库settings的GitHub Pages项下,
选择合适的分支, 作为GitHub Pages site.

## 安装 nodejs ##
Hexo基于Node.js，Node.js下载地址：Download | Node.js 下载安装包，注意安装
Node.js会包含环境变量及npm的安装，安装后，检测Node.js是否安装成功，在命令行中
输入 node -v :

检测npm是否安装成功，在命令行中输入npm -v :

建议更改npm的源:

```
npm config set registry https://registry.npm.taobao.org

```

## 安装 hexo ##

```
npm install -g hexo-cli
```


## 初始化 hexo ##

进入准备建立博客的目录, `hexo init myblog` .

进入 myblog目录, `cd myblog`
文件目录基本如下:

    node_modules: 依赖包
    scaffolds：生成文章的一些模板
    source：用来存放你的文章
    themes：主题
    ** _config.yml: 博客的配置文件**


打开 hexo

```
hexo g
hexo s
```

将hexo和GitHub关联起来，修改站点配置文件_config.yml 其中使用git协议链结仓库,
正确设置ssh密钥的话, 不用再每次都输入密码.

```
deploy:
  type: git
  repo: git@github.com:kangdalei/kangdalei.github.io.git
  branch: master
```

安装deploy-git部署命令
npm install hexo-deployer-git --save

清除之前生成的东西
hexo clean

上传站点
hexo d

## 主题 Next ##
https://github.com/next-theme/hexo-theme-next

Installation

If you're using Hexo 5.0 or later, the simplest way to install is through npm:

$ cd hexo-site
$ npm install hexo-theme-next


启用主题


与所有 Hexo 主题启用的模式一样。 当 克隆/下载 完成后，打开 站点配置文件， 找到
theme 字段，并将其值更改为 next。启用 NexT 主题


```
theme: next
```

## 换行问题 ##
markdown语法以空行来换行, 单纯的一个回车键, 不会换行.  但是hexo默认会把回车键转换为一个<br>


 安装 hexo-renderer-marked
 `npm install hexo-renderer-marked --save`

在站点配置文件里写上:

gfm要关闭.

marked:
  gfm: false
  pedantic: false
  sanitize: false
  tables: true
  breaks: false
  smartLists: true
  smartypants: true
  modifyAnchors: ''
  autolink: true

## hexo 首页只显示部分摘要 ##
https://blog.csdn.net/yueyue200830/article/details/104470646

修改配置

首先需要在Next主题的_config.yml中把设置打开：(默认安装时就打开了)

```
# Automatically excerpt description in homepage as preamble text.
excerpt_description: true
```

之后有两种方法
方法一：写概述

在文章的front-matter中添加description，其中description中的内容就会被显示在首页上，其余一律不显示。

```
---
title: 让首页显示部分内容
date: 2020-02-23 22:55:10
description: 这是显示在首页的概述，正文内容均会被隐藏。
---
```

比较不方便的是还得写一下概述，很多时候会懒得写概述，于是就需要第二种方法了。

方法二：文章截断

在需要截断的地方加入：

``` markdown
<!--more-->
```

首页就会显示这条以上的所有内容，隐藏接下来的所有内容。


## 插件 plugs ##

### footnote 脚注 ###

[github](https://github.com/LouisBarranqueiro/hexo-footnotes)

```
Installation

npm install hexo-footnotes --save

If Hexo detect automatically all plugins, that's all.

If that is not the case, register the plugin in your _config.yml file :

plugins:
  - hexo-footnotes
```



end

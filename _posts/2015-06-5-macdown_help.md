---
layout: post
title: MacDown 基本使用方法
date: 2015-06-05
categories: blog
tags: [工具,知识管理]
description: Mac down的使用方法
---

## MacDown 基本使用方法

![MacDown logo](http://macdown.uranusjr.com/static/images/logo-160.png)

## MacDown 介绍

**Markdown**是由John Gruber创建的纯文本格式化语法，目的是提供一个易于阅读和可行的标记。原始的Markdown语法规范可以在[这里](http://daringfireball.net/projects/markdown/syntax)找到。

**MacDown** 被创建为Markdown文档的一个简单易用的编辑器。我将您的Markdown内容实时转换为HTML，并在预览面板中显示它们。

![MacDown Screenshot](http://d.pr/i/10UGP+)


## 基础

### 换行符
要强制换行，请在行末添加两个空格和换行符（return）。

* This two-line bullet 
won't break

* This two-line bullet  
will break

这里是代码:

```
* This two-line bullet 
won't break

* This two-line bullet  
will break
```

### 强烈和强调

**强**: `**Strong**` 或 `__Strong__` (命令B） 强调  
*Emphasize*: `*Emphasize*` 或 `_Emphasize_`[^emphasize] (命令-I)

### 标题（像这个！）

	Header 1
	========

	Header 2
	--------

要么

	# Header 1
	## Header 2
	### Header 3
	#### Header 4
	##### Header 5
	###### Header 6



### 链接和电子邮件
#### 一致
只需在电子邮件周围加上尖括号，即可点击: <uranusjr@gmail.com>  
`<uranusjr@gmail.com>`  

同样的urls: <http://macdown.uranusjr.com>  
` <http://macdown.uranusjr.com>`  

也许你想要一些链接文本像这样: [Macdown Website](http://macdown.uranusjr.com "Title")  
`[Macdown Website](http://macdown.uranusjr.com "Title")` (标题是可选的)  


#### 参考风格
有时它看起来太乱，以包括大的长url在线，或者你想保持所有的urls在一起。



然后在文件中的任何其他地方的自己的线上创建 [一个链接][arbitrary_id] `[a link][arbitrary_id]`   
`[arbitrary_id]: http://macdown.uranusjr.com "Title"`
  
如果链接文本本身会成为一个好ID，您可以链接 [像这样][] `[like this][]`的文件在其他地方，然后在它自己的行：
`[like this]: http://macdown.uranusjr.com`  

[arbitrary_id]: http://macdown.uranusjr.com "Title"
[like this]: http://macdown.uranusjr.com  


### 图片
#### 像这样
`![Alt Image Text](path/or/url/to.jpg "Optional Title")`
#### 参考风格
`![Alt Image Text][image-id]`  
在它自己的线路其他地方：  
`[image-id]: path/or/url/to.jpg "Optional Title"`


### 列表

* 列表前面必须有空行（或块元素）
* 无序列表用a开始每个项目  `*`
- `-` 工作太
	* 缩进一个级别来创建嵌套列表
		1. 支持有序列表。
		2. 开始每个项目（数字周期空间） like `1. `
		42. 不管你使用什么数字，我都会顺序渲染它们
		1. 所以你可能想开始每一行 `1.` 让我整理出来

这里是代码:

```
* Lists must be preceded by a blank line (or block element)
* Unordered lists start each item with a `*`
- `-` works too
	* Indent a level to make a nested list
		1. Ordered lists are supported.
		2. Start each item (number-period-space) like `1. `
		42. It doesn't matter what number you use, I will render them sequentially
		1. So you might want to start each line with `1.` and let me sort it out
```


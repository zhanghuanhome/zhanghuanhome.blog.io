---
layout: post
title: Android Studio 2.2的新功能？
date: 2016-06-09
categories: blog
tags: [android,工具]
description: 
---

过去几个月是Android Studio的一个激动人心的时刻。首先是版本2.1，支持Android N.然后谷歌I / O带来了我们预览下一个主要版本的Android Studio 2.2预览1的形式，只有这被快速取代了预览2，其中包含一些重要的bug修复和其他改进。

在本文中，我们将进一步了解Android Studio 2.2中的新功能。如果您还没有最新的Android Studio 2.2预览，那么现在是抓住它的最佳时间[下载最新的android studio](http://www.android-studio.org/)

# 1.  新的布局编辑器

Android Studio 2.2中引入的最明显的功能之一是新的布局编辑器。事实上，当您首次启动Android Studio 2.2预览时，您可能会对编辑器看起来有些不同而感到惊讶。

Android Studio的布局编辑器的第一个整体是一个蓝图模式，它暂时隐藏了布局的更精细的细节，所以您可以自由地仔细检查用户界面的间距和排列，而不会分心。要在操作中查看蓝图模式，请确保选择了Android Studio的“ 设计 ”选项卡，然后单击“ 显示蓝图”图标（光标位于下面的屏幕截图中）。

![新的蓝图模式在Android Studio中](https://cms-assets.tutsplus.com/uploads/users/369/posts/26629/image/android-studio-blueprint-mode.jpg)

由于选择了“ **设计** ”选项卡，因此您还应该注意到布局编辑器的另一个新增功能，即右侧的“ **属性**”面板。

选择布局中的任何视图，如果选择类似于或的选项，“  属性”  面板将显示此特定视图的最重要的属性，例如其宽度，contentDescription甚至视图的内容。您可以直接在“ 属性”面板中编辑所有这些属性。TextViewImageView

属性面板显示在Android Studio窗口的右侧
在Android Studio改进的布局编辑器中可以发挥重要作用的最后一个新功能是有点特别。事实上，它是如此特别，它值得自己的部分。我在说ConstraintLayout。

# 2。 ConstraintLayout

ConstraintLayout 是一种灵活的布局管理器，专为Android Studio全新的布局编辑器而设计。

此新布局允许您基于其与布局中其他元素的关系来定义每个视图的布局。这意味着您可以创建复杂的用户界面，而不必求助于嵌套多个布局，这 对于您的应用程序的性能总是坏消息。

如果这一切听起来有点熟悉，那是因为ConstraintLayout从根本上类似RelativeLayout。但是，ConstraintLayout比起来更灵活RelativeLayout，加上它具有额外的好处是被设计为与Android Studio的闪亮的新布局编辑器完美地工作。

可以使ConstraintLayout你最喜欢的布局的秘密酱是约束。约束允许您指定视图相对于其他用户界面元素在屏幕上的位置。例如，将顶部连接  ImageView1到底部，ImageView2意味着ImageView1总是出现在下面ImageView2。您还可以在视图及其父容器之间创建约束。例如，您可以将a的右侧连接TextView 到其父级的右侧边缘ConstraintLayout。

为了帮助您开始使用，Android Studio 2.2的  新建项目向导将  ConstraintLayout 其许多项目模板用作默认布局。

要创建支持的新布局资源文件，  ConstraintLayout请通过右键单击布局目录并选择新建>布局资源文件来创建通常执行的文件。然后，打开此布局文件并将其根目录设置为：

	<?xml version="1.0" encoding="utf-8"?>
	<android.support.constraint.ConstraintLayout xmlns:android="http://	schemas.android.com/apk/res/android"

或者，您可以ConstraintLayout通过打开该布局将任何布局转换为布局，确保选中Android Studio的“ 设计 ”选项卡，右键单击布局，然后从显示的上下文菜单中选择“ 转换为约束布局”。

如果您有一个项目是使用要更新支持的旧版本Android Studio创建的ConstraintLayout，那么您只需要将该ConstraintLayout库添加  到项目的模块级build.gradle文件：

	dependencies {
	...
	...
	compile 'com.android.support.constraint:constraint-layout:1.0.0-alpha2'

一旦你有一个项目和支持的布局资源文件ConstraintLayout，你就可以开始使用约束。

创建约束
新的布局编辑器的推出与  自动连接默认情况下，这意味着Android Studio中启用自动创建的每一个或多个约束查看您添加到您的布局。

要触发自动连接，只需将视图放入ConstraintLayout并拖动该视图即可。一旦你释放那个视图，Autoconnect踢，并创建一些约束与一个漂亮的小动画蓬勃发展。

Autoconnect很方便创建一些快速约束，但它有它的局限性。例如，Autoconnect只能在邻近视图之间创建约束。

如果Autoconnect不创建您所想的那种约束，您可以随时手动添加，删除或编辑约束。如果您决定按照手动路线，通常可以通过选择关闭自动连接图标（光标位于下面的屏幕截图中）提前禁用自动连接。

![使用主蓝图窗口上方的Autconnect图标打开和关闭自动连接模式](https://cms-assets.tutsplus.com/uploads/users/369/posts/26629/image/android-studio-autoconnect.jpg)

要手动创建约束，请选择要向其添加约束的视图。您会发现小圆圈出现在视图的边缘周围。这些是视图的约束句柄。

![将鼠标悬停在视图上，其约束句柄显示为小的白色圆圈](https://cms-assets.tutsplus.com/uploads/users/369/posts/26629/image/android-studio-constraint-handle.jpg)

将鼠标悬停在要创建约束的句柄上，然后单击并拖动。手柄会发出一个箭头，然后你可以拖动到：

* 另一个视图：  向第二个视图拖动手柄。当您正确放置时，将显示“ 要创建...的发布...”工具提示。要创建约束，只需释放句柄。

![拖动约束句柄，它会发出一个箭头](https://cms-assets.tutsplus.com/uploads/users/369/posts/26629/image/android-studio-constraint-view.jpg)


* 父的边缘ConstraintLayout：  将句柄拖向边缘  ConstraintLayout。当您看到要创建的发布...。工具提示，释放句柄以完成约束。

如果向视图添加了相反的约束，那么视图通常会在这两个点之间居中。布局编辑器使用锯齿线显示这些相反的力。

![反对约束显示为锯齿线](https://cms-assets.tutsplus.com/uploads/users/369/posts/26629/image/android-studio-jagged-lines.jpg)

删除约束
当涉及到删除约束，你有几个选择：

* 从特定视图中删除所有约束： 选择视图，然后单击该视图正下方显示的“ 删除约束”图标。

* 从布局中删除所有约束：单击设计窗口正上方的小工具栏中显示的清除所有约束图标。

* 删除单个约束： 选择视图，然后将鼠标悬停在要删除的约束的句柄上。当你看到  点击删除...工具提示，只需给你的鼠标点击，约束将消失在稀薄的空气。

一旦开始使用约束，“ 属性”面板就非常有用。它允许您指定约束的确切大小。

选择视图时，它在“ 属性”面板中显示为正方形，视图的约束表示为线。每个约束都有一个数字，表示约束的长度。

![“属性”面板显示当前选定视图的所有约束](https://cms-assets.tutsplus.com/uploads/users/369/posts/26629/image/android-studio-properties-constraints.jpg)


您可以通过将鼠标悬停在该约束的数字上，直到它变成下拉菜单，来调整每个约束的长度。然后，您可以从该下拉菜单中选择一个新值。

如果选择具有相反约束的视图，“ 属性”面板还将包含一个滑块，您可以使用该滑块沿着这些反对约束的轴定位视图。

# 3. Firebase集成

Firebase是一个全新的服务套件，可帮助您开发高品质的应用程序，从而赢得广大和崇拜的观众。Android Studio预览介绍了Firebase集成，因此您可以在您的项目中添加Firebase服务，而不必离开IDE。

要将Firebase添加到您的项目，请在Android Studio工具栏中单击工具> Firebase 。这将打开新的“ 助理 ”窗口。在此窗口中，您可以单击任何功能以查看有关该特定功能的更多信息，但第一步通常是设置  Firebase Analytics，因为这为探索其他Firebase服务提供了坚实的基础。

使用Firebase Analytics启动和运行的最佳方法是单击“ 助理 ”窗口中的“ Firebase Analytics 入门”链接。这将向您介绍将应用程序连接到Firebase的过程。

# 4. APK分析器

此工具可让您检查原始文件大小和构成APK的每个组件的估计下载大小，从而帮助您缩小APK的大小。利用这些信息，您可以在可以丢失一些多余字节的区域进行调零。APK分析器也可用于检查您的APK是否包含您期望的内容。

要使用APK分析器，请选择构建>分析APK ...，然后选择要仔细查看的APK。然后，APK分析器输出将显示在Android Studio主窗口中，供您浏览组成APK的各种组件。

![APK分析器显示APK的内容以及每个组件的大小](https://cms-assets.tutsplus.com/uploads/users/369/posts/26629/image/android-studio-apk-analyzer.jpg)


5.样品浏览器
如果您在项目中遇到了一堵砖墙，现在您可以转到Android Studio的示例浏览器获取灵感。要访问此功能，请在项目中突出显示一个变量，类型或方法，右键单击，然后从上下文菜单中选择查找示例代码。然后，Android Studio会搜索突出显示的代码在Google的Android代码示例中完成的所有时间，并在主编辑窗口下方的发件箱中显示所有这些匹配项。

6.更多Java 8语言特性

引入Jack工具链意味着您可以在Android项目中开始使用Java 8功能。要启用Java 8语言功能和Jack，请打开项目的模块级build.gradle文件，然后添加以下内容：

	android {
	  ...
	  defaultConfig {
	    ...
	    jackOptions {
	      enabled true
	    }
	  }
	  compileOptions {
	    sourceCompatibility JavaVersion.VERSION_1_8
	    targetCompatibility JavaVersion.VERSION_1_8
	  }
	}
	
有关Android Studio中支持的Java 8功能的更多详细信息，请查看[官方Android文档](https://developer.android.com/preview/j8-jack.html)。

# 7.布局检查器

您可以使用布局检查器来探索和调试布局在Android虚拟设备（AVD）或物理Android设备上运行的快照。要捕获快照，请确保要分析的布局在模拟器或连接的Android设备上可见。接下来，在屏幕底部打开Android Studio的“ Android监视器 ”选项卡，然后选择“ 布局检查器”图标。

# 8.合并清单查看器

通过Android Studio 2.2 中的合并清单查看器，了解您的清单如何与您的项目构建变体中的应用程序依赖关系合并现在更容易。要访问合并清单查看器，请打开项目的AndroidManifest.xml并选择新的合并清单选项卡。

![通过打开您的项目的清单并选择“合并清单”选项卡来访问合并清单查看器](https://cms-assets.tutsplus.com/uploads/users/369/posts/26629/image/android-studio-merged-manifest.jpg)

结论
如果预览是任何事情，那么Android Studio 2.2正在成为Android IDE的一大进步。虽然新功能在现在和最终版本之间可能会发生变化，但它们对Android Studio来说是一个很大的改进，值得投入一些时间来掌握这些新功能。
---
layout: post
title: 开始使用Android VR和Google Cardboard：全景图片
date: 2016-05-01
categories: blog
tags: [android,vr]
description: 
---

在2014年Google I / O大会上，Google推出了Cardboard VR查看器，这是一种由纸板制成的便宜设备，使用镜头和用户手机提供对虚拟现实应用程序的简单访问。两年后，谷歌宣布计划通过销售一个更持久的控制器，被称为白日梦视图，这是建立在相同的概念，使用手机作为主要的VR提供商，在这个平台上扩大。为了获得支持这个平台的更多应用程序，Google发布了Android的Cardboard SDK（标准SDK和NDK），iOS，Unity游戏引擎和虚幻游戏引擎。 

本教程是关于Android Cardboard和Daydream SDK的简短系列的第一篇。在本系列中，我将向您介绍如何使用某些SDK的工具和预制视图，为您的应用添加简单的功能。有很多方法可以将Cardboard SDK集成到您的应用程序的游戏和多媒体！ 

我将集中三个例子，将让你开始在VR开发的世界：一个photosphere查看器，一个360视频查看器和一个输入阅读器的白日梦控制器。我们将在本教程中关注光球观察者，并在后续课程中重温其他两个主题。

## 下载Cardboard SDK并运行示例项目

与Android中的大多数库不同，Android Cardboard SDK在远程存储库中不能正式提供，可以通过Gradle导入为依赖关系。为了使用它，你将需要克隆Android Cardboard SDK从GitHub到您的计算机通过Git。 

>git clone https://github.com/googlevr/gvr-android-sdk.git

下载SDK后，我们先运行其中一个预先打包的示例。在Android Studio启动屏幕上，选择导入项目。接下来，选择您刚刚克隆的SDK的根文件夹，然后按OK。

![Cardboard库的目录结构](https://cms-assets.tutsplus.com/uploads/users/798/posts/27653/image/Screen%20Shot%202016-11-15%20at%202.41.41%20PM.png)

此时，您将可以访问Cardboard SDK for Android中提供的所有库组件和示例。您可以从Android Studio窗口顶部的模块下拉菜单中选择要在设备上运行的示例之一。要确保一切正常工作，请选择**samples-sdk-simplepanowidget**并单击绿色的**Run**箭头。


一旦样本编译和安装，您应该会看到一个关于Machu Picchu的信息屏幕，完整的视图，在移动Android设备时围绕光球旋转。

![纸板全景样品](https://cms-assets.tutsplus.com/uploads/users/798/posts/27653/image/Screenshot_20161118-114402.png)

现在，您可以在设备上运行示例应用程序，让我们开始创建新的支持Cardboard的Android应用程序。

# 创建全景查看器

让我们开始创建一个最低SDK版本为19（KitKat）的新Android项目。在您完成选择和创建一个应用程序模板的标准步骤后，您将需要从Cardboard SDK 库目录将您项目的必需库复制到项目的根文件夹中。对于此示例，请复制以下文件夹：**common，commonwidget和panowidget**。

包括一个新的纸板项目的图书馆
一旦您的库文件在其正确的位置，打开您的项目的settings.gradle文件。您将需要通过此文件将这些库模块添加到您的项目。

>include ':app', ":common", "commonwidget", "panowidget"

接下来，您将需要在节点下的build.gradle文件中引用这些库dependencies，从而允许您访问Cardboard的预制组件。您还需要添加Google的Protocol Buffers JavaNano库，这是一个代码生成和运行时库，以帮助管理Android设备的有限资源。
	
	dependencies {
	    compile fileTree(dir: 'libs', include: ['*.jar'])
	    testCompile 'junit:junit:4.12'
	    compile 'com.android.support:appcompat-v7:25.0.0'
	 
	    compile project(':common')
	    compile project(':commonwidget')
	    compile project(':panowidget')
	 
	    compile 'com.google.protobuf.nano:protobuf-javanano:3.0.0-alpha-7'
	}
	
设置完依赖关系后，打开项目的  AndroidManifest.xml。在文件的顶部，内部节点，您将需要添加和权限纸板SDK。manifestINTERNETREAD_EXTERNAL_STORAGE
	
	<uses-permission android:name="android.permission.INTERNET" />
	<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />

内activity为您的节点MainActivity类，添加  category到intent-filter 了CARDBOARD观众。

	<activity android:name=".MainActivity">
	    <intent-filter>
	        <action android:name="android.intent.action.MAIN" />
	        <category android:name="android.intent.category.LAUNCHER" />
	 
	        <category android:name="com.google.intent.category.CARDBOARD" />
	    </intent-filter>
	</activity>

现在设置过程完成了，我们可以深入到有趣的部分：代码。这个项目将从app / src / main / assets中读取一个Photophere映像，并将其放入VrPanoramaView。您可以通过标准Android相机应用程序拍摄PhotoPhere图像，并将其放入此目录。在这个例子中，我将使用一个名为openspace.jpg的图像  。

![Photosphere在纸板应用程序中显示](https://cms-assets.tutsplus.com/uploads/users/798/posts/27653/image/Screen%20Shot%202016-11-18%20at%201.29.47%20PM1.png)

在您的布局文件中Activity，添加  VrPanoramaView您将在您的应用程序中使用的。

	<?xml version="1.0" encoding="utf-8"?>
	<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent">
	 
	    <com.google.vr.sdk.widgets.pano.VrPanoramaView
	        android:id="@+id/pano_view"
	        android:layout_width="match_parent"
	        android:layout_height="wrap_content" />
	 
	</RelativeLayout>

接下来，打开MainActivity类。您将需要首先获取对您的VrPanoramaViewin 的引用onCreate(Bundle)，然后您可以Bitmap通过loadPhotoSphere()我们将定义的帮助方法加载到它。

	private VrPanoramaView mVrPanoramaView;
	 
	@Override
	protected void onCreate(Bundle savedInstanceState) {
	    super.onCreate(savedInstanceState);
	    setContentView(R.layout.activity_main);
	 
	    mVrPanoramaView = (VrPanoramaView) findViewById(R.id.pano_view);
	 
	    loadPhotoSphere();
	}

loadPhotoSphere()从assets文件夹中检索我们的映像，并将其加载到VrPanoramaView一个VrPanoramaView.Options对象中。值得注意的是，这些图像可能相对较大，所以这个操作通常发生在后台线程，虽然这一课将保持UI线程上的一切简单。

	private void loadPhotoSphere() {
	    //This could take a while. Should do on a background thread, but fine for current example
	    VrPanoramaView.Options options = new VrPanoramaView.Options();
	    InputStream inputStream = null;
	 
	    AssetManager assetManager = getAssets();
	 
	    try {
	        inputStream = assetManager.open("openspace.jpg");
	        options.inputType = VrPanoramaView.Options.TYPE_MONO;
	        mVrPanoramaView.loadImageFromBitmap(BitmapFactory.decodeStream(inputStream), options);
	        inputStream.close();
	    } catch (IOException e) {
	        Log.e("Tuts+", "Exception in loadPhotoSphere: " + e.getMessage() );
	    }
	}

注意，对于VrPanoramaView.Options.inputType值，我们正在使用TYPE_MONO。这意味着，VrPanoramaView期望一个单通道图像进行显示，而一个inputType的  TYPE_STEREO_OVER_UNDER期望与该右垂直分割和左眼看到该图像的顶部和底半部，分别的图像。

![OverUnder的photosphere图像的纸板应用程序](https://cms-assets.tutsplus.com/uploads/users/798/posts/27653/image/andes2.jpg)

你需要做的最后一件事是暂停和恢复渲染VrPanoramaView中onPause()和onResume()，以及正确断开它onDestroy()。

	@Override
	protected void onPause() {
	    mVrPanoramaView.pauseRendering();
	    super.onPause();
	}
	 
	@Override
	protected void onResume() {
	    super.onResume();
	    mVrPanoramaView.resumeRendering();
	}
	 
	@Override
	protected void onDestroy() {
	    mVrPanoramaView.shutdown();
	    super.onDestroy();
	}

现在我们已经完成了应用程序的设置，让我们继续运行它。您的应用程式应该会在萤幕上显示您的相片，而且您应该可以移动手机，查看图片的不同部分。

![我们的全景图像在VR视图外的视图](https://cms-assets.tutsplus.com/uploads/users/798/posts/27653/image/Screenshot_20161118-150640.png)

如果您按下右下角的硬纸板图标View，它会将图像分割成两个稍微偏移的图像，这些图像可以通过Cardboard或Daydream查看器查看。

![我们的全景图像在VR视图里面的视图](https://cms-assets.tutsplus.com/uploads/users/798/posts/27653/image/Screenshot_20161118-150654.png)

虽然在本教程中没有使用，但也值得注意的是，  VrPanoramaView可以接受一个  VrPanoramaEventListener对象，当新图片成功或无法加载时通知您的应用程序。
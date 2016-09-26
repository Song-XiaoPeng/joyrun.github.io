---
title: 利用jitpack.io发布Android、Java项目，通过gradle、maven方式引用
date: 2016-07-21 00:22:52
author:     "Wiki"
tags:
     - 其它
---



> 我们经常使用第三方的开源框架，如果我们写的框架或者工具也比较好用，我们也可以选择开源给别人使用，分享更容易让人成长！

##### 代码管理
毋庸置疑，开源项目的代码管理首选github，这里就不多说了。
### Android、Java项目开源
这里就仅仅介绍如何发布一个Android、Java的项目到 https://jitpack.io  。
#### 第一步：配置项目根目录的build.gradle
```
buildscript {
   repositories {
       jcenter()
   }
   dependencies {
       classpath 'com.android.tools.build:gradle:1.5.0'
       classpath 'com.github.dcendents:android-maven-gradle-plugin:1.3'//ADD
   }
}
```
#### 第二步：配置需要发布的Module Lib的build.gradle
```
apply plugin: 'java'
apply plugin: 'com.github.dcendents.android-maven'//ADD
```
#### 第三步：发布到github

如何发布代码到github这里就不详细说了。

#### 第四步：创建Release版本
1：
![4.1](4.1.jpg)
2：
![4.2](4.2.jpg)
3：
![4.3](4.3.jpg)

截图
#### 第五步：到 https://jitpack.io 构建
![5.1](5.1.jpg)
1. 在输入框输入github的地址，点击Look up就会列出我们创建的发布版本
2. 点击Git it即可开始构建
#### 第六步：完成，即可通过gradle或maven引入发布的Lib
1：
![6.1](6.1.jpg)
2：
![6.2](6.2.jpg)
#### 第六步：添加仓库标识符
把这段Markdown代码放到README.md页面，需要根据项目地址修改。
```
[![](https://jitpack.io/v/joyrun/PullUpSwipeRefreshLayout.svg)](https://jitpack.io/#joyrun/PullUpSwipeRefreshLayout)
```
就可以看到如下效果：
[![](https://jitpack.io/v/joyrun/PullUpSwipeRefreshLayout.svg)](https://jitpack.io/#joyrun/PullUpSwipeRefreshLayout)

#### 第七步：添加开源协议
一个开源项目，通常都是需要选择一个开源协议，通常开源协议的声明在根目录的`LICENSE.txt`，可以在创建项目的时候就选择好开源协议，或者在github直接创建`LICENSE.txt`文件，github会给出提示叫你选择开源协议。

#### 第八步：通过gradle引用

##### gradle引用

修改项目根目录的build.gradle：

```
allprojects {
	repositories {
	//...
	maven { url "https://jitpack.io" }
	}
}
```

修改app的build.gradle：

```
dependencies {
	compile 'com.github.joyrun:PullUpSwipeRefreshLayout:0.1.1'
}
```

##### maven引用

```xml
<repositories>
	<repository>
		<id>jitpack.io</id>
		<url>https://jitpack.io</url>
	</repository>
</repositories>
... 
<dependency>
	<groupId>com.github.joyrun</groupId>
	<artifactId>PullUpSwipeRefreshLayout</artifactId>
	<version>0.1.1</version>
</dependency>
```



#### 示范代码

- 单module项目

  https://github.com/joyrun/PullUpSwipeRefreshLayout


- 多module项目(apt)

  https://github.com/joyrun/AptPreferences
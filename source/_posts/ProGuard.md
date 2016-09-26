---
title: "正确认识Android代码混淆技术"
date: 2016-09-26 00:00:00
author:     "Wiki"
tags:
    - Android
---

Android通常是使用Java语言编写，同时Java是非常容易被反编译的语言，一旦发布出去就基本宣布开源了。为了保证代码的安全性，对代码混淆是非常的重要。从eclipse到Android Studio，很多人对代码混淆并不了解，只是仅仅把混淆用起来而已。

#### ProGuard

ProGuard是一个压缩、优化和混淆Java字节码文件的免费的工具，它可以删除无用的类、字段、方法和属性。可以删除没用的注释，最大限度地优化字节码文件。它还可以使用简短的无意义的名称来重命名已经存在的类、字段、方法和属性。常常用于Android开发用于混淆最终的项目，增加项目被反编译的难度。

#### Android自带的混淆规则

Android的SDK带有两个混淆规则文件，proguard-android.txt和proguard-android-optimize.txt，后面会介绍他们之间的区别，他们都是存放在sdk的tools/proguard文件夹中，如果有使用混淆，建议认真阅读这两个文件，非常的重要。

### Keep Options

##### -keep

指定类和类的成员（属性和方法）不被混淆。

##### -keepclassmembers

 保护指定类的成员（属性和方法）不被混淆。

##### -keepclasseswithmembers

保护指定的类和类的成员，但条件是所有指定的类和类成员是要存在。 

##### -keepnames

保护指定的类和类的成员的名称（保证他们不会压缩步骤中删除） 

##### -keepclassmembernames

保护指定的类的成员的名称（保证他们不会压缩步骤中删除） 

##### -keepclasseswithmembernames

保护指定的类和类的成员的名称，如果所有指定的类成员出席（在压缩步骤之后） 

### 常见案例

##### 避免反射Javabean类被混淆

我们的项目中通常都会用到Gson和ORM数据库，他们都是通过反射来系列化和反系列化对象，一旦被混淆，字段名和方法名都改变，就会运行出错。避免这种javabean类被混淆通常有两种方法，第一种就是指定包名下所有的类都不混淆，通常javabean类都是放在同一个文件夹。第二种方式就是指定继承或实现某个类或接口的类不混淆，比如Serializable、Parcelable。

指定包名下类不被混淆

```
-keep class co.runner.app.bean.** {*;}
```

指定实现某接口类不被混淆

```
-keep class * implements java.io.Serializable { *; }
-keep class * implements android.os.Parcelable { *; }
```

##### 指定方法不混淆：避免属性动画的方法被混淆

属性动画也是通过反射来实现，如果指定整个类都不混淆是非常的不划算。我们可以仅仅指定某个方法不混淆。

```
-keepclassmembers class com.app.MainActivity {
	void set*(***);
}
```

### 避免注解被混淆

我们在使用反射同时也常用注解，比如EventBus 8.0之前的版本，



### 通过注解@Keep避免被混淆

我们查看proguard-android.txt和proguard-android-optimize.txt可以发现，默认的混淆规则中包含了





### optimize

optimize可以对代码进行优化，删除无用的代码，甚至可以按照要求去删除代码，比如生产环境的Log日志，可以最大限度地优化字节码文件。对ProGuard来说，没有被调用的代码就是无用代码，我们有很多的代码是通过反射调用，所以使用optimize一定要通过大量的测试，以确保没有问题。proguard-android.txt混淆规则默认是-dontoptimize，意思就是不打开optimize。如果我们需要使用optimize，那么就需要使用proguard-android-optimize.txt规则。



#### 过滤Log日志

通常在正式发布版本中，我们并不需要输出Log日志，通过混淆optimize技术可以在混淆过程中删除Log代码，可以避免通过Log看到一些私密的数据。如果需要过滤Log日志，那么就必须使用proguard-android-optimize.txt规则，并且不能有`-dontoptimize`。

```
-assumenosideeffects class android.util.Log {
    public static *** v(...);
    public static *** d(...);
    public static *** i(...);
    public static *** w(...);
    public static *** e(...);
}
```



参考的混淆规则


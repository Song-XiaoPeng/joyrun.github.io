---
layout:     post
title:      "Java Annotation最佳入门实践"
date:       2016-07-18
author:     "Wiki"
tags:
    - Java
---



### Annotation

#### 元注解：
元注解的作用就是负责注解其他注解。Java5.0定义了4个标准的meta-annotation类型，它们被用来提供对其它 annotation类型作说明。Java5.0定义的元注解：
1. @Target,
2. @Retention,
3. @Documented,
4. @Inherited

这些类型和它们所支持的类在java.lang.annotation包中可以找到。下面我们看一下每个元注解的作用和相应分参数的使用说明。
##### @Target：
> 规定Annotation所修饰的对象范围。
1. ElementType.CONSTRUCTOR:构造器声明
2. ElementType.FIELD:成员变量、对象、属性（包括enum实例）
3. ElementType.LOCAL_VARIABLE:局部变量声明
4. ElementType.METHOD:方法声明
5. ElementType.PACKAGE:包声明
6. ElementType.PARAMETER:参数声明
7. ElementType.TYPE:类、接口（包括注解类型)或enum声明

使用实例：

```
@Target(ElementType.TYPE)
public @interface Table {
    /**
     * 数据表名称注解，默认值为类名称
     * @return
     */
    public String tableName() default "className";
}
```
@Target还支持填写数组，同时支持多种范围
```
@Target({ElementType.TYPE,ElementType.FIELD})
public @interface Test {}
```
##### @Retention：
对Annotation的“生命周期”限制：某些Annotation仅出现在源代码中，而被编译器丢弃；而另一些却被编译在class文件中；编译在class文件中的Annotation可能会被虚拟机忽略，而另一些在class被装载时将被读取。
> 作用：表示需要在什么级别保存该注释信息，用于描述注解的生命周期（即：被描述的注解在什么范围内有效）
1. RetentionPolicy.SOURCE:在源文件中有效
2. RetentionPolicy.CLASS:在class文件中有效
3. RetentionPolicy.RUNTIME:在运行时有效

使用实例：
```
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Column {
    public String name() default "fieldName";
    public boolean defaultDBValue() default false;
}
```
> Column注解的的RetentionPolicy的属性值是RUTIME,这样注解处理器可以通过反射，获取到该注解的属性值，从而去做一些运行时的逻辑处理。
##### @Documented:
用于描述其它类型的annotation应该被作为被标注的程序成员的公共API，因此可以被例如javadoc此类的工具文档化。Documented是一个标记注解，没有成员。

```
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Column {
    public String name() default "fieldName";
}
```
##### @Inherited：
是一个标记注解，@Inherited阐述了某个被标注的类型是被继承的。如果一个使用了@Inherited修饰的annotation类型被用于一个class，则这个annotation将被用于该class的子类。

> 注意：当@Inherited的Retention是RetentionPolicy.RUNTIME，则反射API增强了这种继承性。如果我们使用java.lang.reflect去查询一个@Inherited annotation类型的annotation时，反射代码检查将展开工作：检查class和其父类，直到发现指定的annotation类型被发现。

```
@Inherited
public @interface Greeting {
    public enum FontColor{ BULE,RED,GREEN};
    String name();
    FontColor fontColor() default FontColor.GREEN;
}
```
##### 注解参数
参数成员只能用基本类型byte,short,char,int,long,float,double,boolean八种基本数据类型和 String,Enum,Class,annotations等数据类型,以及这一些类型的数组。
##### 默认值
注解元素必须有确定的值，要么在定义注解的默认值中指定，要么在使用注解时指定，非基本类型的注解元素的值不可为null。因此, 使用空字符串或0作为默认值是一种常用的做法。

```
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface FruitProvider {
    public int id() default -1;
    public String name() default "";
    public String address() default "";
}
```


```
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface FruitProvider {
    public int value() default -1;
}
```


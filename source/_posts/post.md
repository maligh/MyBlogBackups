---
title: iOS常量(const)、enum、宏(#define)的使用场景及区别
date: 2016.04.30 02:37:54
tags: 
	- 宏 
	- 常量
categories: Objective-C
photos:
- http://upload-images.jianshu.io/upload_images/1457495-df0b95375604d34b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240
---


>前言：本文主要梳理iOS中如何使用常量、enum、宏，以及各自的使用场景。

## 重要的事情首先说：
在iOS开发中请尽量多使用const、enum来代替宏定义(#define)；随着项目工程的逐渐增大，过多的宏定义还可能影响项目的运行速度（这点待考证）

* 宏定义大家应该都不陌生，使用起来非常简单，首先我们先来看一下宏定义跟const的区别：
1.宏在编译开始之前就会被替换，而const只是变量进行修饰;
2.宏可以定义一些函数方法，const不能
3.宏编译时只替换不做检查不报错，也就是说有重复定义问题。而const会编译检查，会报错
---
## 那到底什么时候使用宏，什么时候该使用const？

<!-- more -->
 * 定义`不对外公开的常量`的时候，我们应该尽量先考虑使用 static 方式声名const来替代使用宏定义。const不能满足的情况再考虑使用宏定义。比如用以下定义：
```objectivec
static NSString * const kConst = @"Hello"；
static const CGFloat kWidth = 10.0;
```
代替：
```objectivec
#define DEFINE @"Hello"
#define WIDTH 10.0
```
  * 当定义`对外公开的常量`的时候，我们一般使用如下定义：
```objectivec
//Test.h
extern NSString * const CLASSNAMEconst;
//Test.m
NSString * const CLASSNAMEconst = @"hello";
```

 * 对于整型类型，代替宏定义直接定义整型常量比较好的办法是使用enum，使用enum时推荐使用NS_ENUM和NS_OPTIONS宏。比如用以下定义：
```objectivec
typedef NS_ENUM(NSInteger,TestEnum) {
        MY_INT_CONST = 12345
};
```
代替：
```objectivec
#define MY_INT_CONST 12345
```
NS_OPTIONS定义方式如下：
```objectivec
typedef NS_OPTIONS(NSInteger, SelectType) {
        SelectA    = 0,
        SelectB    = 1 << 0,
        SelectC    = 1 << 1,
        SelectD    = 1 << 2
};
```
<!--readmore-->

---

## 下面顺便说一下const 的一些使用方式，主要说明这几种写法的区别：
```objectivec
const NSString *constString1 = @"I am a const NSString * string";
NSString const *constString2 = @"I am a NSString const * string";
static const NSString *staticConstString1 = @"I am a static const NSString * string";
static NSString const *staticConstString2 = @"I am a static NSString const * string";
NSString * const stringConst = @"I am a NSString * const string";
```
---
全局变量：
 ```objectivec
//全局变量，constString1地址不能修改，constString1值能修改
const NSString *constString1 = @"I am a const NSString * string";
//意义同上，无区别
NSString const *constString2 = @"I am a NSString const * string";
// stringConst 地址能修改，stringConst值不能修改
NSString * const stringConst = @"I am a NSString * const string";
```
constString1 跟constString2 无区别.
＊左边代表指针本身的类型信息，const表示这个指针指向的这个地址是不可变的
＊右边代表指针指向变量的可变性，即指针存储的地址指向的内存单元所存储的变量的可变性

---

局部常量：
```objectivec
//作用域只在本文件中
static const NSString *kstaticConstString1 = @"I am a static const NSString * string";
static NSString const *kstaticConstString2 = @"I am a static NSString const * string";
//---------------------------
```
---
## 总结：
不要用宏定义定义常量，能用const,enum替换的以后就少用宏定义吧。有任何问题或者指点请直接留言，欢迎拍砖~最后感谢你的时间~



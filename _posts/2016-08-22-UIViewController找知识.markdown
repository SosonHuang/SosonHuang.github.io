---
layout:     post
title:      "UIViewController找知识"
subtitle:   "UIViewController找知识"
date:       2016-08-22
author:     "Soson"
header-img: "img/home-bg-geek.jpg"
tags:
    - 基础知识
---



> UIViewController是iOS开发当中最常用的一个类，在里面找知识！

进去UIViewController，可以看到 NS_ASSUME_NONNULL_BEGIN，最底部可以看到 NS_ASSUME_NONNULL_END，这个是干嘛用？

![](https://raw.githubusercontent.com/SosonHuang/imagesource/master/1/1.jpg)

《__nullable && ___nonnull》__nullable指代对象可以为null或者为nil，__nonnull指代对象不能为null。当不遵循这规则时，编译器会给出警告。

``` 
@interface SosonClass ()
@property (nonatomic, copy) NSArray * items;
- (id)itemWithName:(NSString * __nonnull)name;
@end
```

``` 
 @implementation SosonClass
- (void)test {
  [self itemWithName:nil];    // 编译器警告：Null passed to a callee that requires a non-null argument
}
- (id)itemWithName:(NSString * __nonnull)name {
    return nil;
}
@end
```

__nullable && ___nonnull还可以去掉下划线nullable和nonnull。

如果需要每个属性或每个方法都去指定`nonnull`和`nullable`，是一件非常繁琐的事。苹果为了减轻我们的工作量，专门提供了两个宏：`NS_ASSUME_NONNULL_BEGIN`和`NS_ASSUME_NONNULL_END`。在这两个宏之间的代码，所有简单指针对象都被假定为nonnull，因此我们只需要去指定那些nullable的指针，如下：

``` objective-c
NS_ASSUME_NONNULL_BEGIN
@interface SosonClass ()
@property (nonatomic, copy) NSArray * items;
- (id)itemWithName:(nullable NSString *)name;
@end
NS_ASSUME_NONNULL_END
```

items属性默认是`non null`的，itemWithName:方法的返回值也是`non null`，而参数是指定为`nullable`的！

------

``` objective-c
@classUIView; 
@class UINavigationItem, UIBarButtonItem, UITabBarItem;
@class UISearchDisplayController;
@class UIPopoverController;
@class UIStoryboard, UIStoryboardSegue, UIStoryboardUnwindSegueSource;
@class UIScrollView;
```

可以看到很多使用@class修饰的类，在此简述一下#import #include与@class之间的区别：

> 1.使用#include的时候要注意处理重复引用的问题，重复引用会报错



> 2.#import 不会引起交叉编译,确保头文件只会被导入一次，尽量食用＃improt，不要食用include



> 3.@class只是告诉编译器有这么一个类，引用类名，至于这些类是如何定义实现的不去考虑，一般用在.h文件的@interface之前



> 4.@class比#import编译效率更高。此外@class和#import的主要区别在于解决引用死锁的问题

如果有一个头文件a.h，在其他大量头文件中都需要引用头文件a.h，如果使用的是#import，那么当a.h中有了一点改动时，其他包含a.h的头文件都需要重新编译，这将耗费大量的时间，降低了开发效率，而如果在需要的时候使用的是@class，当a.h中有了一点改动时，由于其他头文件并没有将a.h的内容包含进来，就不用重新编译，提高了开发效率，还有一个用法会引起编译错误的就是在a.h中#import ‘’b.h‘’ 在b.h中#import ‘’a.h‘’那么在编译的时候也会出现错误。

``` objective-c
typedef NS_ENUM(NSInteger, UIModalTransitionStyle) {
    UIModalTransitionStyleCoverVertical = 0,
    UIModalTransitionStyleFlipHorizontal __TVOS_PROHIBITED,
    UIModalTransitionStyleCrossDissolve,
    UIModalTransitionStylePartialCurl NS_ENUM_AVAILABLE_IOS(32) _TVOS_PROHIBITED,
};
typedef NS_ENUM(NSInteger, UIModalPresentationStyle) {
        UIModalPresentationFullScreen = 0,
        UIModalPresentationPageSheet NS_ENUM_AVAILABLE_IOS(32) _TVOS_PROHIBITED,
        UIModalPresentationFormSheet NS_ENUM_AVAILABLE_IOS(32) _TVOS_PROHIBITED,
        UIModalPresentationCurrentContext NS_ENUM_AVAILABLE_IOS(3_2),
        UIModalPresentationCustom NS_ENUM_AVAILABLE_IOS(7_0),
        UIModalPresentationOverFullScreen NS_ENUM_AVAILABLE_IOS(8_0),
        UIModalPresentationOverCurrentContext NS_ENUM_AVAILABLE_IOS(8_0),
        UIModalPresentationPopover NS_ENUM_AVAILABLE_IOS(80) _TVOS_PROHIBITED,
        UIModalPresentationNone NS_ENUM_AVAILABLE_IOS(7_0) = -1,         
};
```



这是弹出框相关定义的动画枚举。

> UIModalPresentationCurrentContext NS_ENUM_AVAILABLE_IOS(3_2), 表示3.2以上有此枚举



> UIModalPresentationCustom NS_ENUM_AVAILABLE_IOS(7_0),            表示7.0以上有此枚举



> UIModalPresentationOverFullScreen NS_ENUM_AVAILABLE_IOS(8_0), 表示8.0以上有此枚举

所以在开发中需要注意，如果是兼容7.0的版本，要记住不要让7.0调用8.0的枚举，否则会crash！

往下看，iOS 8上随着Size Class概念的提出，UIViewController支持了UIContentContainer这样一组新的协议

![](https://raw.githubusercontent.com/SosonHuang/imagesource/master/1/2.jpg)

UIViewController默认实现这组协议。自定义ViewController的时候可以重写这些方法来调整视图布局，我们可以在这些方法里调整ChildViewControler的位置，具体的方法使用已经有英文注释，大家可以google找用法，直接看官方文档翻译也行！

![](https://raw.githubusercontent.com/SosonHuang/imagesource/master/1/3.jpg)

 看到上面代码联想到iOS中的宏和const，我们来看看区别：

宏常定义字符串和代码

> 字符串：#define SERVICEERROR @"数据出错"



> 代码：#define RGBA(r, g, b, a) [UIColor colorWithRed:(float)r / 255.0f green:(float)g / 255.0f blue:(float)b / 255.0f alpha:a]

常量const定义字符串

> 苹果推荐我们使用const，苹果经常把常用字符串定义成const



> 被const关键字修饰的变量是只读的



> 定义全局变量

- `编译时刻`:宏是预编译（编译之前处理），const是编译阶段。
- `编译检查`:宏不做检查，不会报编译错误，只是替换，const会编译检查，会报编译错误。
- `宏的好处`:宏能定义一些函数，方法。 const不能。
- `宏的坏处`:使用大量宏，容易造成编译时间久，每次都需要重新替换

``` objective-c
static  NSString * const key = @"name";
```

> static声明的常量，static是静态存储类型，属于局部变量，只能用于同一个函数内，在其他函数内使用是错误的！



> extern是外部存储类型，属于全局变量，可以用于从他定义开始的后续所有函数内！

上图中可以看到采用 UIKIT_EXTERN来修饰常量, 进去UIKIT_EXTERN可以发现，使用的是extern！

``` objective-c
#define UIKIT_EXTERN            extern attribute((visibility ("default")))
```

所以以后最好采用这种方式定义常量！

![](https://raw.githubusercontent.com/SosonHuang/imagesource/master/1/4.jpg)

``` 
- (nullable instancetype)initWithCoder:(NSCoder *)aDecoder NS_DESIGNATED_INITIALIZER;
```

当从文件（xib）加载UIView的时候，initWithCoder会执行，当从代码实例化UIView的时候，initWithFrame会执行！

> NS_DESIGNATED_INITIALIZER（特定构造方法）：子类如果重写了父类的特定构造方法, 那么必须使用super调用父类的特定构造方法！

UIViewController生命周期：

> loadView ：访问UIViewController的view属性时调用，一般初始化View，目前很多的View初始化采用懒加载



> viewDidLoad ：视图加载完成调用，一般处理数据请求



> viewWillAppear ：视图即将可见时调用



> viewDidAppear ：视图已经出现是调用



> viewWillLayoutSubviews ：视图布局将要发生改变时调用



> viewDidLayoutSubviews :视图布局已经发生改变完成调用

在以下情况下会被调用：

``` 
1、init初始化不会触发layoutSubviews
2、addSubview会触发layoutSubviews
3、设置view的Frame会触发layoutSubviews，当然前提是frame的值设置前后发生了变化
4、滚动一个UIScrollView会触发layoutSubviews
5、旋转Screen会触发父UIView上的layoutSubviews事件
6、改变一个UIView大小的时候也会触发父UIView上的layoutSubviews事件
```

> viewWillDisappear：当前视图将要消失的时候调用



> viewDidDisappear ：当前视图已经消失的时候调用

``` 
当一个视图控制器被创建，并在屏幕上显示的时候。 代码的执行顺序

1、alloc创建对象，分配空间
2、init (initWithNibName) 初始化对象，初始化数据
3、loadView从nib载入视图 ，通常这一步不需要去干涉。除非你没有使用xib文件创建视图
4、viewDidLoad载入完成，可以进行自定义数据以及动态创建其他控件
5、viewWillAppear视图将出现在屏幕之前，马上这个视图就会被展现在屏幕上了
6、viewDidAppear视图已在屏幕上渲染完成当一个视图被移除屏幕并且销毁的时候的执行顺序

这个顺序差不多和上面的相反
1、viewWillDisappear视图将被从屏幕上移除之前执行
2、viewDidDisappear视图已经被从屏幕上移除，用户看不到这个视图了
3、dealloc视图被销毁，此处需要对你在init和viewDidLoad中创建的对象进行释放
```



![](https://raw.githubusercontent.com/SosonHuang/imagesource/master/1/5.jpg)

> UIViewController的类别UIViewController (UIViewControllerRotation)，主要定义了旋转的一些方法操作！

![](https://raw.githubusercontent.com/SosonHuang/imagesource/master/1/6.jpg)

> UIViewController的类别UIViewController (UIViewControllerEditing)，支持编辑，在导航栏加入编辑的bar！

![](https://raw.githubusercontent.com/SosonHuang/imagesource/master/1/8.jpg)

> UIViewController的类别UIViewController (UISearchDisplayControllerSupport)，支持SearchDisplay！

![](https://raw.githubusercontent.com/SosonHuang/imagesource/master/1/9.jpg)

> UIViewController的类别UIViewController (UIContainerViewControllerProtectedMethods)，支持删除和添加子视图！

![](https://raw.githubusercontent.com/SosonHuang/imagesource/master/1/10.jpg)

> UIViewController的类别UIViewController (**UIConstraintBasedLayoutCoreMethods**)，新增了一个更新布局约束的方法





**苹果开发着官方文档：[https://developer.apple.com/reference/uikit?language=objc](https://developer.apple.com/reference/uikit?language=objc)   学以致用！**


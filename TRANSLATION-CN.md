iOS 最佳实践
==================

## 译者注

本文翻译自 [futurice 公司](http://www.futurice.com/)的 [iOS Good Practices](https://github.com/futurice/ios-good-practices)，译文在 [Github](https://github.com/oa414/ios-good-practices) 上进行维护，同时在 [简书](http://www.jianshu.com/p/b0bf2368fb95) 上进行发布。

本文尚未经过大牛审校，纰漏瑕疵以及语句不顺的地方，以在 [Github](https://github.com/oa414/ios-good-practices) 提出，请大家多多指正。

以下是正文

--------------

_就像软件一样，如果我们不持续改进这份文档，它就会落伍。我们希望大家都来帮助我们改进它 —— 只要开一个 issue 或者发送一个 pull request!_

对其他平台感兴趣？看看我们的 [Android 开发最佳实践][android-best-practices] 和 [Windows App 开发最佳实践][windows-app-development-best-practices]吧。

[android-best-practices]: https://github.com/futurice/android-best-practices
[windows-app-development-best-practices]: https://github.com/futurice/windows-app-development-best-practices

## 为什么阅读本文档

跳进了 iOS 的坑真是麻烦。无论是 Swift 还是 Objective-C， 都没有在其他地方广泛使用，而且这个平台对每个东西都几乎有它自己的命名方式，并且连要在真机上调试都充满了坎坷。无论你是刚刚入门 Cocoa 还是想纠正自己开发习惯的开发者，都能从本文档获益。不过下面写的仅仅是建议，所以如果你有一个更好的方案，那就试试吧！

## 入门

### Xcode

[Xcode][xcode] 是大多数 iOS 开发者的选择，并且是 Apple 唯一官方支持的 IDE。有一些其他的选择，比如 [AppCode][appcode] 是最有名的，但是除非你是经验丰富的开发者，否则就使用 Xcode 吧。不要管它的一些小缺点啦，它现在已经蛮好用咯。

要安装 Xcode，只需要下载 [Mac App Store 中的 Xcode ][xcode-app-store]。它会同时下载最新的 SDK 和模拟器，同时你可以从 _Preferences > Downloads_ 下载更多的内容。

[xcode]: https://developer.apple.com/xcode/
[appcode]: https://www.jetbrains.com/objc/
[xcode-app-store]: https://itunes.apple.com/us/app/xcode/id497799835


### 项目初始化

开始 iOS 开发的时候，一个常见的问题是用代码写所有的 view 还是使用 Interface Builder（Storyboards 或者 XIB ）。两种方案都久经考量。但是，有下面几个考虑：

#### 为什么使用代码？

* Storyboard 因为它的复杂的 XML 结构容易带来版本冲突，这让代码合并变得困难。
* 容易用代码结构化以及复用 view，让你的代码变得 [DRY][dry]。
* 所有的信息汇集一处。在 Interface Builder 里面你需要点击所有的检查器来寻找你要找的东西。


[dry]: http://en.wikipedia.org/wiki/Don%27t_repeat_yourself

#### 为什么用 Storyboard？

* 为了更少的技术要求，Storyboard 使用了一个很好的直接贡献于项目的方法，比如，通过调整颜色或者布局的 constraints。然而，它要求一个项目已经做好配置，并且开发者有一些时间掌握基础
* 当你不用构建项目也能看到变化的时候，集成更快了
* 在 Xcode6 里面，自定义的文字和 UI 元素在 storyboard 里面都可以可视化表示，比你在代码里面修改好多了
* 从 iOS8 开始，[Size Classes][size-classes] 允许你设计不同类型设备的屏幕，不用重复一些工作

[size-classes]: http://blog.futurice.com/adaptive-view-ios8

### 忽略文件

当把项目放入版本控制系统的时候，首先应该有一个好的 `.gitignore` 文件。这样，不必要的文件（用户设置，临时文件这些）都不会放进你的仓库里面。幸运的是，Github 已经给了我们  [Objective-C][objc-gitignore] 和 [Swift][swift-gitignore] 语言的模板

[objc-gitignore]: https://github.com/github/gitignore/blob/master/Objective-C.gitignore
[swift-gitignore]: https://github.com/github/gitignore/blob/master/Swift.gitignore

### CocoaPods

如果你计划增加外部依赖(比如，第三方库)在你的项目中，[CocoaPods][cocoapods] 提供了一个快捷的途径，就像这样：

    sudo gem install cocoapods

要开始使用，仅仅需要在你的 iOS 项目目录下运行：

    pod init

它会创建一个 Podfile，会管理你所有的依赖，在 Podfile 中加入你的依赖后，运行

    pod install

来安装第三方库并且将它们作为 workspace 的一部分，你的 workspace 也会包含你自己的项目。 一般推荐[提交你自己的项目的依赖][committing-pods]，而不是每个开发者在一个 checkout 之后运行 `pod install` 。

注意在之后，你需要打开 `.xcworkspace`  而不是 `.xcproject`，否则你的代码就不能被编译了，命令：

    pod update

会升级所有的 pod 到最新版本，你可以用大量 [符号][cocoapods-pod-syntax] 来定义你期望的版本需求。

[cocoapods]: http://www.cocoapods.org
[cocoapods-pod-syntax]: http://guides.cocoapods.org/syntax/podfile.html#pod
[committing-pods]: https://www.dzombak.com/blog/2014/03/including-pods-in-source-control.html

### 项目结构

为了组织目录里面的上百个源代码文件，最好根据你的架构来设置一些文件结构。比如，你可以用这样的：

    ├─ Models
    ├─ Views
    ├─ Controllers
    ├─ Stores
    ├─ Helpers


首先，将他们创建为 Group（黄色的目录），在 Xcode 的项目导航里面的你的项目中。然后对每个项目里的文件，将它们连接到真实的文件目录 —— 通过打开它右边的文件检查器，点击小小的目录图标，在你的项目目录下创建一个和 group 同名的子目录。

#### 本地化

一开始就应该把所有的显示给用户的字符串放进本地化文件。这样不仅仅为了翻译方便，同时也便于查找用户看见的文本。你可以在 build Scheme 中加入一个启动参数来指定特定的语言

    -AppleLanguages (Finnish)


对于更复杂的翻译问题，比如复数（比如 "1 person" 和  "3 people"），你应该使用 [`.stringsdict` format][stringsdict-format]  而不是一个普通的  `localizable.strings` 文件。在有了这个强大的工具来处理比如  "one", some", "few" 和 "many" 的情况。 [当处理俄罗斯语和阿拉伯语的时候][language-plural-rules]，你就不用急得抓耳挠腮了。

关于本地化的更多信息，看 2012年 2月的 Helsink iOS 会议的 [幻灯片][l10n-slides]，大部分内容对现在也是适用的。

[stringsdict-format]: https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/StringsdictFileFormat/StringsdictFileFormat.html
[language-plural-rules]: http://www.unicode.org/cldr/charts/latest/supplemental/language_plural_rules.html
[l10n-slides]: https://speakerdeck.com/hasseg/localization-practicum

####  常量

创建被 prefix header 引入的一个  `Constants.h` 文件。

不要用宏定义（用 `#define`），用实际的常量定义

    static CGFloat const XYZBrandingFontSizeSmall = 12.0f;
    static NSString * const XYZAwesomenessDeliveredNotificationName = @"foo";

常量类型安全，并且有更明确的作用域（并不是在所有没有被引入的文件中能使用）。不能被重定义，并且可以在调试器中使用。


###  分支模型

特别是分发你的 app 的时候（如提交到 App Store），最好把分支用特别的 tag 区分。同时，新特性的开发，会引入很多 commit 的，也应该在独立的分支上提交。[`git-flow`][gitflow-github] 是一个帮助你遵从这个约定的工具。它简单地包装了 git 的分支和 tag 的命令，帮助维护一个正确的分支结构。所有的开发分支（比如  `develop`）， 关于 app 版本的 tag 分支以及提交到 master 分支的：

    git flow release finish <version>

[gitflow-github]: https://github.com/nvie/gitflow

## 常用库

总得来说，来把一个第三方库加入到你的项目中需要慎重考虑。确实，一个灵巧的库或许能帮助你解决现在的问题，但是可能在之后陷入维护的噩梦，比如在下一个系统版本改变一些东西之后，或者一个第三方库的场景变成了官方的 API。不过在一个良好的设计的代码中，切换实现是很轻松的。尽量考虑用 Apple 的广泛（而且优秀的）框架吧～

这个小节尽量保持简短。库的特性是为了减少模板代码（比如 AutoLayout）或者解决复杂的需要很多测试的问题，比如日期计算。当你在 iOS 中更加专业的时候，挖掘这里的源代码，通过它们底层的 Apple 框架来认识他们，你会发现这些可以做大量工作。

### AFNetworking

99.95% 的 iOS 开发者使用这个网络库，当 `NSURLSession` 自己本身也非常完善的时候， `AFNetworking` 仍然能凭借很多 app 需要的队列请求管理功能立足于不败之地。

### DateTools 日期工具

总得来说，[不要自己写日期计算][timezones-youtube]。幸运的是，有一个 DateTools 是 MIT 协议的，而且经过彻底测试的，你可以放心的在需要用日期的时候使用它。

[timezones-youtube]: https://www.youtube.com/watch?v=-5wpm-gesOY

###  Auto Layout 库

如果你更喜欢在代码里面写界面，你会用过 Apple 难用的 'NSLayoutConstraint'  的工厂或者 [Visual Format Language][visual-format-language] 。前者很罗嗦，后者基于字符串，不利于编译器检查。


[Masonry][masonry-github] 通过它们自己的 DSL 来取代常量， Swift 中一个类似的库是 [Cartography][cartography-github]，它利用了语言的丰富的操作符重载特性。如果更加保守的话，[FLKAutoLayout][flkautolayout-github] 提供了一个干净但是没有太多魔法的原生 API 包装。

[visual-format-language]: https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/VisualFormatLanguage/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH3-SW1
[masonry-github]: https://www.github.com/Masonry/Masonry
[cartography-github]: https://github.com/robb/Cartography
[flkautolayout-github]: https://github.com/floriankugler/FLKAutoLayout

## Architecture 架构


* [Model-View-Controller-Store (MVCS)][mvcs]
    * 这是 Apple 默认的架构（MVC），通过扩展一表示 Model 实例的存储层以及处理网络，缓存等内容。
    * 每一个存储通过 `RACSignal`s 或者   `void` 返回值的自定义 block 方法来返回。
* [Model-View-ViewModel (MVVM)][mvvm]
    * 通过  "massive view controllers": MVVM 认为 `UIViewController` 子类是 view 的一部分，并且保持精简，通过在 viewmodel 里面维持状态。 
    * 对于 Cocoa 开发者是非常新的概念，但是 [获得][cocoasamurai-rac] [推动][raywenderlich-mvvm]
* [View-Interactor-Presenter-Entity-Routing (VIPER)][viper]
    * 值得在大型项目中一看的架构，在 MVVM 都显得复杂，而且需要关注测试的时候。

[mvcs]: http://programmers.stackexchange.com/questions/184396/mvcs-model-view-controller-store
[mvvm]: http://www.objc.io/issue-13/mvvm.html
[cocoasamurai-rac]: http://cocoasamurai.blogspot.de/2013/03/basic-mvvm-with-reactivecocoa.html
[raywenderlich-mvvm]: http://www.raywenderlich.com/74106/mvvm-tutorial-with-reactivecocoa-part-1
[viper]: http://www.objc.io/issue-13/viper.html

###  事件模式

这里有一些通知其他对象的常用方法：


* __Delegation （委托）:__ _(一对一)_ Apple 经常用它（或者说，太多了）。用它来执行回调，比如， Model View 做一个回调
* __Callback blocks （回调代码块）:__ _(一对一)_ 可以更加解耦，可以维护类似的相关代码段。同时在有很多 sender 的时候比委托有更好的扩展性。
* __Notification Center （通知）:__ _(一对多)_ 最常用的对象来向第一个观察者发送事件的方法。非常解耦合 - 通知甚至可以全局地进行观察，而不用引用派发对象
* __Key-Value Observing (KVO，键值编码):__ _(一对多)_ 不需要观察者来明确发送的时间，就像 _Key-Value Coding (KVC)_  符合观察的键（属性）。通常不推荐使用，因为他不自然的特性以及繁琐的API。
* __Signals（信号）:__ _(一对多)_  [ReactiveCocoa][reactivecocoa-github] 的核心, 允许链接和组合你的内容, 提供了防止 [callback hell][elm-escape-from-callback-hell] 的一个方法。


[elm-escape-from-callback-hell]: http://elm-lang.org/learn/Escape-from-Callback-Hell.elm

### Models

保持你的 model 是不可变的，以及用他们来改变远程的 API 的语义以及类型。Github 的 [Mantle](https://github.com/Mantle/Mantle) 是一个好的选择

### Views

当用 Auto Layout 布局你的 view 的时候，确保在你父类中加入了下面的代码：

    + (BOOL)requiresConstraintBasedLayout
    {
        return YES;
    }

否则当系统没有调用 `-updateConstraints` 的时候，你可能会遇到奇怪的 bug。

### Controllers

建议使用依赖注入，比如：传递任何需要的对象作为参数，而不是在一个单例中保持所有的状态。后一种方法仅仅在状态是 _真的_ 全局的时候适用。
 
```objective-c
+ [[FooDetailsViewController alloc] initWithFoo:(Foo *)foo];
```

##  网络

### 传统的方式：使用回调 block

```objective-c
// GigStore.h

typedef void (^FetchGigsBlock)(NSArray *gigs, NSError *error);

- (void)fetchGigsForArtist:(Artist *)artist completion:(FetchGigsBlock)completion


// GigsViewController.m

[[GigStore sharedStore] fetchGigsForArtist:artist completion:^(NSArray *gigs, NSError *error) {
    if (!error) {
        // Do something with gigs
    }
    else {
        // :(
    }
];
```

这样运行，但是如果你有多个组合的网络请求的时候，就会进入回调嵌套的地狱。

###  Reactive 的方法： 使用 RAC 信号

如果你发现已经陷入了回调的地狱，看看 [ReactiveCocoa (RAC)][reactivecocoa-github] 吧，它是一个万能而且多功能的库，能改变大家写 [整个 app][groceryList-github] 的方法，但是你可以在它适用的地方保守地使用它。

在 [Teehan+Lax][teehan-lax-rac] 和 [NSHipster][nshipster-rac] 上面有一些关于 RAC (and FRP in general) 以及函数式编程的概念的优秀介绍。

[grocerylist-github]: https://github.com/jspahrsummers/GroceryList
[teehan-lax-rac]: http://www.teehanlax.com/blog/getting-started-with-reactivecocoa/
[nshipster-rac]: http://nshipster.com/reactivecocoa/

```objective-c
// GigStore.h

- (RACSignal *)gigsForArtist:(Artist *)artist;


// GigsViewController.m

[[GigStore sharedStore] gigsForArtist:artist]
    subscribeNext:^(NSArray *gigs) {
        // Do something with gigs
    } error:^(NSError *error) {
        // :(
    }
];
```

它允许通过信号和其他信号的结合，在展示它们之前做一些改变。

## Assets 资源

[Asset catalogs][asset-catalogs] 是管理你所有项目可视化资源的最好方式，它们可以同时管理通用的以及设备相关的 (iPhone 4-inch, iPhone Retina, iPad, 等) 资源，并且会自动通过它们的名字分组。告诉你的设计师如何添加它们（Xcode 有内置的 Git 客户端）可以节省很多时间，否则你会花很多时间来在从邮件或者其他渠道复制到代码库里面。它可以让设计师马上尝试资源改变的样子，并且反复实验。

[asset-catalogs]: https://developer.apple.com/library/ios/recipes/xcode_help-image_catalog-1.0/Recipe.html

### Using Bitmap Images  使用位图


Asset catalogs 仅仅暴露了图片的名称，图片集里面的抽象的名字。这可以避免资源名字的冲突，就像 `button_large@2x.png` 的文件的命名空间在它的图片集里面。遵守一些命名规则可以让生活更美好：

```objective-c
IconCheckmarkHighlighted.png // Universal, non-Retina
IconCheckmarkHighlighted@2x.png // Universal, Retina
IconCheckmarkHighlighted~iphone.png // iPhone, non-Retina
IconCheckmarkHighlighted@2x~iphone.png // iPhone, Retina
IconCheckmarkHighlighted-568h@2x~iphone.png // iPhone, Retina, 4-inch
IconCheckmarkHighlighted~ipad.png // iPad, non-Retina
IconCheckmarkHighlighted@2x~ipad.png // iPad, Retina
```

修饰后缀 `-568h`, `@2x`, `~iphone` and `~ipad` 是不必要的，但是有他们在文件里面，当把文件拖进去的时候，Xcode 会正确地处置它们。这避免赋值错误。

### 使用向量图

你可以用设计师原始的 [vector graphics (PDFs)][vector-assets] 加入到  asset catalogs，Xcode 可以自动地根据它们生成位图。这减少了你的工程的复杂性（管理更少的文件）。

[vector-assets]: http://martiancraft.com/blog/2014/09/vector-images-xcode6/

## 代码风格

### 命名

尽管命名约定很长，但是 Apple 一如既往地在 API 中遵守了命名原则。

[cocoa-coding-guidelines]: https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html

这里有一些你应该使用的基本原则：

一个方法用 _动词_ 开头的表示它做了一些副作用，但是不会返回任何东西：
`- (void)loadView;`
`- (void)startAnimating;`


任何以 _名词_ 开头的方法，没有副作用而且返回了它指的对象：
`- (UINavigationItem *)navigationItem;`
`+ (UILabel *)labelWithText:(NSString *)text;`

尽量分离两者，比如，不要在操作数据的时候产生副作用，反之亦然。这会让你的副作用包含到代码更小的细分粒度里面，让调试变得非常困难。

### 结构

[Pragma marks](http://nshipster.com/pragma/) 是把你代码分组的一个好方法，特别是在 view Controller 里。下面的结构可以适用于绝大多数 view Controller 的工作：

```objective-c

#import "SomeModel.h"
#import "SomeView.h"
#import "SomeController.h"
#import "SomeStore.h"
#import "SomeHelper.h"
#import <SomeExternalLibrary/SomeExternalLibraryHeader.h>

static NSString * const XYZFooStringConstant = @"FoobarConstant";
static CGFloat const XYZFooFloatConstant = 1234.5;

@interface XYZFooViewController () <XYZBarDelegate>

@property (nonatomic, copy, readonly) Foo *foo;

@end

@implementation XYZFooViewController

#pragma mark - Lifecycle

- (instancetype)initWithFoo:(Foo *)foo;
- (void)dealloc;

#pragma mark - View Lifecycle

- (void)viewDidLoad;
- (void)viewWillAppear:(BOOL)animated;

#pragma mark - Layout

- (void)makeViewConstraints;

#pragma mark - Public Interface

- (void)startFooing;
- (void)stopFooing;

#pragma mark - User Interaction

- (void)foobarButtonTapped;

#pragma mark - XYZFoobarDelegate

- (void)foobar:(Foobar *)foobar didSomethingWithFoo:(Foo *)foo;

#pragma mark - Internal Helpers

- (NSString *)displayNameForFoo:(Foo *)foo;

@end
```


最重要的一点是在你项目的类中保持一致性

###  其他风格指南

我们公司没有任何公司级别的代码风格指南，详细看看其他开发者的 Objective-C 风格指南很有用，即使一些内容是公司相关的或者过于激进了。


* [GitHub](https://github.com/github/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [The New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Ray Wenderlich](https://github.com/raywenderlich/objective-c-style-guide)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [Luke Redpath](http://lukeredpath.co.uk/blog/2011/06/28/my-objective-c-style-guide/)

##  诊断

### 编译警告


推荐你尽可能多打开编译警告，并且像对待错误一样对待编译警告。推荐 [这个PPT][warnings-slides]。这个幻灯片覆盖了如何在特定文件，或者特别代码段里面消除相关警告的内容。


简单的来说，至少需要在 _“Other Warning Flags” 编译设置里面定义下面的值：

- `-Wall` _(增加很多的警告)_
- `-Wextra` _(增加更多的警告)_


同时打开 _“Treat warnings as errors”_ 

[warnings-slides]: https://speakerdeck.com/hasseg/the-compiler-is-your-friend

###  Clang 静态分析


Clang 编译器（Xcode 使用的）有一个 _静态分析器_ 来进行你的代码控制和数据流的分析，来检测编译器不能检测的许多错误。

你可以通过在 Xcode 里面手动运行 _Product → Analyze_ 菜单项来手动执行代码分析


分析器可以用浅或者深的模式运行，后者更加慢，但是可以从跨函数的控制流和数据流上分析更多问题


推荐：

- 打开 _所有_ 分析器检查 (通过在 building setting 中打开所有 “Static Analyzer” 选项)
- 在 release 的编译设置里面打开 _“Analyze during ‘Build’”_ 来让分析器自动在发布的版本构建的时候运行。(这样你就不需要记住要手动运行了)
- 把 _“Mode of Analysis for ‘Analyze’”_ 设置为 _Shallow (faster)_
- 把 _“Mode of Analysis for ‘Build’”_ 设置为 _Deep_


### [Faux Pas](http://fauxpasapp.com/)

我们自己的 [Ali Rantakari][ali-rantakari-twitter] 创建的，Faux Pas 是一个极佳的静态错误检测工具，它分析你的代码并且找出那些你自己甚至都没发现的问题。在提交你的 App 到应用商店前用它吧！

[ali-rantakari-twitter]: https://twitter.com/AliRantakari

### 调试

当你的 App 崩溃的时候，Xcode 不会默认进入到调试器里面。为了调试，你需要增加一个异常断点（在 Xcode 的 Debug 导航中点 “+”），来在异常发生的时候退出执行。在很多情况下，你需要看看触发这些异常的代码。它会捕捉任何异常，即使是已经处理的。如果 Xcode 在一个第三方库里面中断执行，比如，你可能需要通过选择 _Edit Breakpoint_ 并且设置 _Exception_ 为 _Objective-C_ 。

对于视图 debug，[Reveal][reveal] 和 [Spark Inspector][spark-inspector] 这两个强有力的可视化检查工具可以帮你省下很多时间，特别是在你使用 Auto Layout 并且希望定位出问题或者溢出屏幕的视图的时候。Xcode 提供了免费的[类似功能][xcode-view-debugging]，但是只能适用于 iOS 8+ 并且不那么好用。

[reveal]: http://revealapp.com/
[spark-inspector]: http://sparkinspector.com
[xcode-view-debugging]: https://developer.apple.com/library/ios/recipes/xcode_help-debugger/using_view_debugger/using_view_debugger.html

### 分析

Xcode 有一个叫 Instruments 的分析工具，它包括了许多内存分析，CPU，网络通讯，图形以及更多的工具，它有点复杂的，但是它的追踪内存泄漏的时候还是蛮直观的。只需要在 Xcode 中 选择 _Product_ > _Profile_ ，选择 Allocations， 点击 Record 按钮并且用一些有用的字符串过滤申请空间的信息，比如你自己的 app 的类名。它会在固定的列中统计，并且告诉你每个对象有多少实例。到底是什么类一直增加实例导致内存泄漏。

Instruments 也有自动化的工具来进行录制并且运行 UI 交互以及 JavaScript 文件。[UI Auto Monkey][ui-auto-monkey] 是一个自动化随机点击、滑动以及旋转你的 app 的脚本，他在压力、渗透测试中很有用。

[ui-auto-monkey]: https://github.com/jonathanpenn/ui-auto-monkey

## 统计

强烈推荐使用一些统计框架，他们直观地告诉你有多少人用你的应用。X 特性增加了用户么？Y 按钮很难找到么？为了回答这些问题，你可以发送事件，运行时间以及其他记录的信息到一个聚集它们并且可视化它们的服务。比如，[Google Tag Manager][google-tag-manager]。这个是一个比 Google Analytics 更加有用的地方是可以在 app 和统计之间插入数据，所以数据逻辑可以通过 web 服务进行修改，而不用更新你的 app。

[google-tag-manager]: http://www.google.com/tagmanager/

一个好的实践是创建一个简单的 helper 类，比如 `XYZAnalyticsHelper`，处理 app 内部 model 以及数据格式 (XYZModel, NSTimeInterval, …)的变换，来适配字符串为主的数据层，

```objective-c

- (void)pushAddItemEventWithItem:(XYZItem *)item editMode:(XYZEditMode)editMode
{
    NSString *editModeString = [self nameForEditMode:editMode];

    [self pushToDataLayer:@{
        @"event": "addItem",
        @"itemIdentifier": item.identifier,
        @"editMode": editModeString
    }];
}

```


另外的优点是，你在必要的时候可以替换整个统计框架，而不用改变 app 其他部分。

### Crash Logs 崩溃日志

应该让你的 app 向一个服务发送崩溃日志。你可以手动实现，通过 [PLCrashReporter][plcrashreporter] 以及你自己的后端。但是强烈推荐你使用现有的服务，比如下面的

* [Crashlytics](http://www.crashlytics.com)
* [HockeyApp](http://hockeyapp.net)
* [Crittercism](https://www.crittercism.com)
* [Splunk MINTexpress](https://mint.splunk.com)

[plcrashreporter]: https://www.plcrashreporter.org

当你配置好后，确保你 _保存了 the Xcode archive (`.xcarchive`)_  对于每一个 app 放出的版本。这个归档中包含了构建的 app 的二进制以及调试符号(`dSYM`)，你需要用每个版本特定的 app 把你的 Crash 报告符号化。

## 构建

### 构建设置

每一个简单的 app 都可以不同的方式构建，最基本的分离是 Xcode 给你 _debug_ 和 _release_  之间的构建方案。后者在编译的时候有更多的优化，可能会导致你需要多调试一些问题。Apple 建议你在开发的时候用 _debug_ 模式，在打包的时候用 _release_ 设置。这是默认的 Scheme（Play 和 Stop 后面的下拉菜单），运行 Run 的时候会用 _debug_ 设置而运行 Archive 的时候会使用 _release_ 。


然后，对于真实的 app 这似乎太简单了。你可能，不，是 [_应该_][futurice-environments]！有不同的测试环境，staging 和其他相关的开发活动。每一个可能有自己的 URL，日志级别，bundle ID）所以你可以一起安装它们，以及描述文件。然后一个简单的 debug/release 区别不能分离这些，你可以在你项目的设置中的 Info 选项卡做更多的编译设置。

[futurice-environments]: https://blog.futurice.com/five-environments-you-cannot-develop-without

#### 关于构建设置的 `xcconfig` 文件 

通常构建设置是 Xcode GUI 定义的，但是你同样可以用 _configuration settings files_ (“`.xcconfig` files”)，优点是：


- 你可以注释
- 你可以 `#include` 其他构建设置文件, 能帮助你减少重复:
    - 如果你有一些适用于所有构建设置的设置, 增加一个 `Common.xcconfig` 并且在其他构建设置文件里面 `#include` 
    - 如果你，比如，希望有一个 “Debug” 构建设置文件，允许编译器优化，你只需要 `#include "MyApp_Debug.xcconfig"` 来重载其他设置
- 冲突解决和合并变得更轻松

更多关于这个主题的信息请看[这个 PPT][xcconfig-slides]。

[xcconfig-slides]: https://speakerdeck.com/hasseg/xcode-configuration-files

### Targets

一个 target 是处于比项目更低一级的级别。比如，一个项目可能有多个 target，可能重载它的项目设置。简单地说，每个 target 和一个 app 相当。比如，你可能有几个因为国家区分的 app（从同样的代码编译）来提交到 App Store。每个会有 development/staging/release 的构建，所以最好通过构建设置而不是 target 来区分。一个 app 只有一个 target 是很少见的。

### Schemes

Schemes 告诉 Xcode 在你点击 Run, Test, Profile, Analyze 或者 Archive 操作的时候应该怎么做。它们把这些操作映射到一个 target 和一个构建设置中。你可以传递启动参数，比如 app 需要运行的语言（为了测试本地化）或者一些了为了调试用的诊断标志。


一个建议的 Scheme 命名是 `MyApp (<Language>) [Environment]`:

    MyApp (English) [Development]
    MyApp (German) [Development]
    MyApp [Testing]
    MyApp [Staging]
    MyApp [App Store]

对于大多数环境来说语言部分是不必要的，app 会可能会以非 Xcode 的方式安装，比如用 TestFlight， 启动参数会被忽略。这个情况下，为了测试本地化需要手动设置设备的语言。

## 部署


部署一个软件到 iOS 设备上并不直观。但是有一些核心观点，只要理解了，对你有很大的帮助。

### 签名

当你需要在真实设备上运行软件的时候，你需要用一个 Apple 认证的 __证书__ 签名。每一个证书是连接到一个公、私密钥对，私钥会保存在你的 Mac 的 KeyChain 里面，证书有两种类型

* __开发证书:__ 每个组的开发者都有自己的证书，而且它通过请求特到。Xcode 可以帮你完成，但是最好不用点击 "Fix issue" 来完成，而是理解它到底做了什么事情。在部署开发版本到设备上的时候需要这个证书。
* __发布证书:__ 可以有多个，但是最好每一个组织有一个，并且通过内部渠道共享。在提交 App 到 App Store 或者你的企业的内部 App Store 的时候需要这个证书。

###  描述文件


除了证书，还有 __描述文件__ ， 它把设备和证书连接起来。而且，它分成开发和发布两种类型。


* __Development provisioning profile__: 开发描述文件， 它包含了一个包含所有能安装这个 app 的设备列表。它连接了一个或者多个开发者允许这个描述文件使用的证书。描述文件可以确认特定的 app，但是对于大多数开发目的，它特别适合用一个通配符描述文件，也就是 Apple ID 以一个星号 (*) 结尾的。


* __Distribution provisioning profile:__ 发布描述文件本身，对于不同使用目的也有不同的类型。每一个发布描述文件链接到一个发布证书，并且在证书过期的时候失效。
    * __Ad-Hoc:__ 就像开发证书一个，它包含了一个 app 可以安装的设备的白名单。这个描述文件可以用来做 beta 版本，每年可以测试 100 个设备。为了做更细致的测试以及升级到 1000 个测试用户，你可以使用 Apple 最近发布的  [TestFlight][testflight] 服务，Supertop 提供了一个 [summary of its advantages and issues][testflight-discussion].
    * __App Store:__  这个 profile 没有列出设备，任何人都可以通过苹果官方的发布来安装，所有 App Store 发布的 App 都需要这个证书
    * __Enterprise:__ 就像 App Store，没有设备白名单，任何通过企业内部网络的人都可以从内部“应用商店”安装。应用商店可能只是一个带连接的网站。这个描述文件只允许企业账号使用。




[testflight]: https://developer.apple.com/testflight/
[testflight-discussion]: http://blog.supertop.co/post/108759935377/app-developer-friends-try-testflight

想同步所有的证书和描述文件到你的机器，可以去 Xcode 的  Preferences 的 Accounts 下，添加你的 Apple ID，然后双击你的 Team 名称。然后底部会有一个刷新按钮，有时候你需要重启 Xcode 来让东西显示出来。

####  调试描述文件



有时候你需要调试一个描述文件的问题。比如，Xcode 可能拒绝安装 app 到一个设备中，因为设备没有在开发或者 Ad-doc 发布的的描述文件设备列表中。这个情况下，你可以使用 Craig Hockenberry 的 [Provisioning][provisioning] 插件，浏览 `~/Library/MobileDevice/Provisioning Profiles`，选择一个 `.mobileprovision` 文件，并用空格键调用 Finder 的快速查看特性，它会告诉你关于设备，entitlements，证书以及 Apple ID 的信息。

[provisioning]: https://github.com/chockenberry/Provisioning

###  上传


[iTunes Connect][itunes-connect] 是你管理上传到 App Store 的 App 的后台网站。 Xcode 6 需要一个 Apple ID 作为开发者账号来签名并且上传二进制。如果你有多个开发者账号而且要上传它们的 app 就需要一点技巧。 因为 _一个 Apple ID 只能和一个 iTunes Connect 账号关联_ ，一个变通方案是为每个 iTunes Connect 账号创建一个新的　Apple ID，使用 Application Loader 取代 Xcode 来上传应用。这样有效地解除了上传最终 `.app` 文件构建和签名的耦合。

在上传二进制后，耐心等待，可能要花上一个小时。当你的 App 出现后，可以链接到对应的 App 版本并且提交审核

[itunes-connect]: https://itunesconnect.apple.com

## 应用内购买

当验证一个 App 内购的 receipt 的时候，记住做以下步骤：

- __Authenticity:__  receipt 是来自 Apple 的
- __Integrity:__  receipt 没有被篡改
- __App match:__ receipt 的 App bundle ID 和你的 App bundle ID 一致 
- __Product match:__ receipt 里面产品 ID 和你期望的一致 
- __Freshness:__ 你之前没有验证一样的 receipt ID


如果可能，把你的 IAP 存储销售相关的内容存储在服务器端，并且只在一个合法的经过上述检查的 receipt。这样的设计避免了常见的盗窃机制，同时，因为服务器做了验证，所以你可以使用 Apple 的 HTTP 验证服务来取代你自己的 `PKCS #7` / `ASN.1` 格式。

更多关于这个主题的信息可以看 [Futurice blog: Validating in-app purchases in your iOS app][futu-blog-iap]

[futu-blog-iap]: http://futurice.com/blog/validating-in-app-purchases-in-your-ios-app

[reactivecocoa-github]: https://github.com/ReactiveCocoa/ReactiveCocoa

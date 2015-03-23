iOS最佳实践
==================

_Just like software, this document will rot unless we take care of it. We encourage everyone to help us on that – just open an issue or send a pull request!_

_就像软件，如果我们不持续提示它的话这份文档就会落伍。我们希望大家都来帮助我们提升它 —— 只要开一个 issue 或者发送一个  pull request!_


Interested in other mobile platforms? Our [Best Practices in Android Development][android-best-practices] and [Windows App Development Best Practices][windows-app-development-best-practices] documents have got you covered.

对其他平台感兴趣？看看我们的 [Best Practices in Android Development][android-best-practices] and [Windows App Development Best Practices][windows-app-development-best-practices]

[android-best-practices]: https://github.com/futurice/android-best-practices
[windows-app-development-best-practices]: https://github.com/futurice/windows-app-development-best-practices

## Why? 为什么

Getting on board with iOS can be intimidating. Neither Swift nor Objective-C are widely used elsewhere, the platform has its own names for almost everything, and it's a bumpy road for your code to actually make it onto a physical device. This living document is here to help you, whether you're taking your first steps in Cocoaland or you're curious about doing things "the right way". Everything below is just suggestions, so if you have a good reason to do something differently, by all means go for it!

跳进了 iOS 的坑真是麻烦。无论是 Swift 还是  Objective-C 都没有在其他地方广泛使用，而且这个平台对每个东西都几乎有它自己的命名，并且连让你的代码在真实的设备上跑起来都充满了坎坷。这个文档希望帮追，无论你是刚刚入门 Cocoa 还是抱着纠正自己开发习惯的心。下面列出的仅仅是建议的，所以如果你有一个更好的方案，那就试试吧！

## Getting Started 入门

### Xcode

[Xcode][xcode] is the IDE of choice for most iOS developers, and the only one officially supported by Apple. There are some alternatives, of which [AppCode][appcode] is arguably the most famous, but unless you're already a seasoned iOS person, go with Xcode. Despite its shortcomings, it's actually quite usable nowadays!

To install, simply download [Xcode on the Mac App Store][xcode-app-store]. It comes with the newest SDK and simulators, and you can install more stuff under _Preferences > Downloads_.


[Xcode][xcode] 是大多数 iOS 开发者的选择，并且是 Apple 唯一官方支持的。有一些其他的选择，比如 [AppCode][appcode] 是最有名的，但是除非你是一个经验丰富的 iOS 开发者，那就使用 Xcode 吧。不要管它的一些小缺点啦，它现在已经蛮好用咯。

要安装 Xcode，只需要下载 [Xcode on the Mac App Store][xcode-app-store]。它会同时下载最新的 SDK 和模拟器，同事你可以从 _Preferences > Downloads_ 下载更多的内容。

[xcode]: https://developer.apple.com/xcode/
[appcode]: https://www.jetbrains.com/objc/
[xcode-app-store]: https://itunes.apple.com/us/app/xcode/id497799835


### Project Setup 项目初始化

A common question when beginning an iOS project is whether to write all views in code or use Interface Builder with Storyboards or XIB files. Both are known to occasionally result in working software. However, there are a few considerations:

一个常见的开始iOS开发的问题是用代码写所有的 view 还是使用 Interface Builder（Storyboards 或者 XIB ）。两个都在现实环境中经过使用。然后，有下面几个考量：

#### Why code? 为什么用代码
* Storyboards are more prone to version conflicts due to their complex XML structure. This makes merging much harder than with code.
* It's easier to structure and reuse views in code, thereby keeping your codebase [DRY][dry].
* All information is in one place. In Interface Builder you have to click through all the inspectors to find what you're looking for.


* Storyboard 因为它的复杂的 XML 结构容易带来版本冲突，这让代码合并变得困难。
* 容易用代码结构化以及复用 view，让你的代码变得 [DRY][dry]。
* 所有的信息汇集一处。在 Interface Builder 里面你需要点击所有的检查器来寻找你要找的东西。


[dry]: http://en.wikipedia.org/wiki/Don%27t_repeat_yourself

#### Why Storyboards? 为什么用 Storyboards？
* For the less technically inclined, Storyboards can be a great way to contribute to the project directly, e.g. by tweaking colors or layout constraints. However, this requires a working project setup and some time to learn the basics.
* Iteration is often faster since you can preview certain changes without building the project.
* In Xcode 6, custom fonts and UI elements are finally represented visually in Storyboards, giving you a much better idea of the final appearance while designing.
* Starting with iOS 8, [Size Classes][size-classes] allow you to design for different device types and screens without duplication.

[size-classes]: http://blog.futurice.com/adaptive-view-ios8

### Ignores

A good first step when putting a project under version control is to have a decent `.gitignore` file. That way, unwanted files (user settings, temporary files, etc.) will never even make it into your repository. Luckily, GitHub has us covered for both [Objective-C][objc-gitignore] and [Swift][swift-gitignore].

[objc-gitignore]: https://github.com/github/gitignore/blob/master/Objective-C.gitignore
[swift-gitignore]: https://github.com/github/gitignore/blob/master/Swift.gitignore

### CocoaPods

If you're planning on including external dependencies (e.g. third-party libraries) in your project, [CocoaPods][cocoapods] offers easy and fast integration. Install it like so:

如果你计划增加外部依赖(比如，第三方依赖)在你的项目中， [CocoaPods][cocoapods] 提供了一个快捷的途径，就像这样：

    sudo gem install cocoapods

To get started, move inside your iOS project folder and run

开始使用，仅仅需要在你的 iOS 项目目录下运行：

    pod init

This creates a Podfile, which will hold all your dependencies in one place. After adding your dependencies to the Podfile, you run

它会创建一个 Podfile， 会管理你所有的依赖，在 Podfile 中加入你的依赖后，允许

    pod install

to install the libraries and include them as part of a workspace which also holds your own project. It is generally [recommended to commit the installed dependencies to your own repo][committing-pods], instead of relying on having each developer running `pod install` after a fresh checkout.

来安装第三非苦并且将它们作为 workspace 的一部分，你的workspace 也会包含你自己的项目。 一般推荐[提交你自己的项目的依赖][committing-pods]，而不是依赖每个开发者运行 `pod install` 在一个 checkout之后。

Note that from now on, you'll need to open the `.xcworkspace` file instead of `.xcproject`, or your code will not compile. The command

注意在之后你需要打开 `.xcworkspace`  而不是 `.xcproject`，否则你的代码就不能被编译了，命令：

    pod update

will update all pods to the newest versions permitted by the Podfile. You can use a wealth of [operators][cocoapods-pod-syntax] to specify your exact version requirements.

会升级所有的 pod 到最新版本，你可以用大量  [符号][cocoapods-pod-syntax] 来定义你期望的版本需求。

[cocoapods]: http://www.cocoapods.org
[cocoapods-pod-syntax]: http://guides.cocoapods.org/syntax/podfile.html#pod
[committing-pods]: https://www.dzombak.com/blog/2014/03/including-pods-in-source-control.html

### Project Structure 项目结构

To keep all those hundreds of source files ending up in the same directory, it's a good idea to set up some folder structure depending on your architecture. For instance, you can use the following:

为了组织目录里面的上百个源代码文件，最好根据你的架构来设置一些文件结构。比如，你可以用这样的：

    ├─ Models
    ├─ Views
    ├─ Controllers
    ├─ Stores
    ├─ Helpers

First, create them as groups (little yellow "folders") within the group with your project's name in Xcode's Project Navigator. Then, for each of the groups, link them to an actual directory in your project path by opening their File Inspector on the right, hitting the little gray folder icon, and creating a new subfolder with the name of the group in your project directory.

首先，将他们创建为 Group（黄色的目录），用 Xcode 的项目导航里面的你的项目中。然后对每个项目里的文件，将它么连接到真实的文件目录，通过打开右边的他们的文件检查器，点击小小的目录图标，创建一个和group同名的子目录，在你的项目目录下。

#### Localization

Keep all user strings in localization files right from the beginning. This is good not only for translations, but also for finding user-facing text quickly. You can add a launch argument to your build scheme to launch the app in a certain language, e.g.



    -AppleLanguages (Finnish)

For more complex translations such as plural forms that depending on a number of items (e.g. "1 person" vs. "3 people"), you should use the [`.stringsdict` format][stringsdict-format] instead of a regular `localizable.strings` file. As soon as you've wrapped your head around the crazy syntax, you have a powerful tool that knows how to make plurals for "one", some", "few" and "many" items, as needed [e.g. in Russian or Arabic][language-plural-rules].

Find more information about localization in [these presentation slides][l10n-slides] from the February 2012 HelsinkiOS meetup. Most of the talk is still relevant in October 2014.

[stringsdict-format]: https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/StringsdictFileFormat/StringsdictFileFormat.html
[language-plural-rules]: http://www.unicode.org/cldr/charts/latest/supplemental/language_plural_rules.html
[l10n-slides]: https://speakerdeck.com/hasseg/localization-practicum

#### Constants 常量

Keep app-wide constants in a `Constants.h` file that is included in the prefix header.

创建被 prefix header 引入的一个  `Constants.h` 

Instead of preprocessor macro definitions (via `#define`), use actual constants:
不要用宏定义（用 `#define`），用实际的常量

    static CGFloat const XYZBrandingFontSizeSmall = 12.0f;
    static NSString * const XYZAwesomenessDeliveredNotificationName = @"foo";


Actual constants are type-safe, have more explicit scope (they’re not available in all imported/included files until undefined), cannot be redefined or undefined in later parts of the code, and are available in the debugger.

常量是类型安全，并且有更明确的作用域（并不是在所有没有被引入的包含文件中能用的）。不能被重定义，并且可以在调试器中使用。


### Branching Model 分支模型

Especially when distributing an app to the public (e.g. through the App Store), it's a good idea to isolate releases to their own branch with proper tags. Also, feature work that involves a lot of commits should be done on its own branch. [`git-flow`][gitflow-github] is a tool that helps you follow these conventions. It is simply a convenience wrapper around Git's branching and tagging commands, but can help maintain a proper branching structure especially for teams. Do all development on feature branches (or on `develop` for smaller work), tag releases with the app version, and commit to master only via

特别是分发你的 app 的时候（如提交到 App Store），虽好把分支用特别的 tag 隔离。同时，新特性的工作，会引入很多 commits的，也应该在独立的分支上提交。[`git-flow`][gitflow-github] 是一个帮助你遵从这个约定的工具。它简单地包装了 git的分支和tag的命令，帮助维护一个正确的分支结构。所有的开发分支（比如  `develop`）， 关于 app版本的tag分支以及提交到 master分支的：

    git flow release finish <version>

[gitflow-github]: https://github.com/nvie/gitflow

## Common Libraries 常用库

Generally speaking, make it a conscious decision to add an external dependency to your project. Sure, this one neat library solves your problem now, but maybe later gets stuck in maintenance limbo, with the next OS version that breaks everything being just around the corner. Another scenario is that a feature only achievable with external libraries suddenly becomes part of the official APIs. In a well-designed codebase, switching out the implementation is a small effort that pays off quickly. Always consider solving the problem using Apple's extensive (and mostly excellent) frameworks first!

总得给来说， make it a conscious decision 来把一个第三方库加入到你的项目中

Therefore this section has been deliberately kept rather short. The libraries featured here tend to reduce boilerplate code (e.g. Auto Layout) or solve complex problems that require extensive testing, such as date calculations. As you become more proficient with iOS, be sure to dive into the source here and there, and acquaint yourself with their underlying Apple frameworks. You'll find that those alone can do a lot of the heavy lifting.

### AFNetworking

A perceived 99.95 percent of iOS developers use this network library. While `NSURLSession` is surprisingly powerful by itself, `AFNetworking` remains unbeaten when it comes to actually managing a queue of requests, which is pretty much a requirement in any modern app.

99.95% 的 iOS开发者使用这个网络库，当 `NSURLSession` 自己本身也非常完善的时候， `AFNetworking` 仍然能凭借很多app需要的队列请求管理功能立足于不败之地。

### DateTools 日期工具
As a general rule, [don't write your date calculations yourself][timezones-youtube]. Luckily, in DateTools you get an MIT-licensed, thoroughly tested library that covers pretty much all your calendary needs.

[timezones-youtube]: https://www.youtube.com/watch?v=-5wpm-gesOY

### Auto Layout Libraries   Auto Layout 库
If you prefer to write your views in code, chances are you've met either of Apple's awkward syntaxes – the regular 'NSLayoutConstraint' factory or the so-called [Visual Format Language][visual-format-language]. The former is extremely verbose and the latter based on strings, which effectively prevents compile-time checking.

[Masonry][masonry-github] remedies this by introducing its own DSL to make, update and replace constraints. A similar approach for Swift is taken by [Cartography][cartography-github], which builds on the language's powerful operator overloading features. For the more conservative, [FLKAutoLayout][flkautolayout-github] offers a clean, but rather non-magical wrapper around the native APIs.

[visual-format-language]: https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/VisualFormatLanguage/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH3-SW1
[masonry-github]: https://www.github.com/Masonry/Masonry
[cartography-github]: https://github.com/robb/Cartography
[flkautolayout-github]: https://github.com/floriankugler/FLKAutoLayout

## Architecture 架构

* [Model-View-Controller-Store (MVCS)][mvcs]
    * This is the default Apple architecture (MVC), extended by a Store layer that vends Model instances and handles the networking, caching etc.
    * Every Store exposes to the view controllers either `RACSignal`s or `void`-returning methods with custom completion blocks
* [Model-View-ViewModel (MVVM)][mvvm]
    * Motivated by "massive view controllers": MVVM considers `UIViewController` subclasses part of the View and keeps them slim by maintaining all state in the ViewModel
    * Quite new concept for Cocoa developers, but [gaining][cocoasamurai-rac] [traction][raywenderlich-mvvm]
* [View-Interactor-Presenter-Entity-Routing (VIPER)][viper]
    * Rather exotic architecture that might be worth looking into in larger projects, where even MVVM feels too cluttered and testability is a major concern

[mvcs]: http://programmers.stackexchange.com/questions/184396/mvcs-model-view-controller-store
[mvvm]: http://www.objc.io/issue-13/mvvm.html
[cocoasamurai-rac]: http://cocoasamurai.blogspot.de/2013/03/basic-mvvm-with-reactivecocoa.html
[raywenderlich-mvvm]: http://www.raywenderlich.com/74106/mvvm-tutorial-with-reactivecocoa-part-1
[viper]: http://www.objc.io/issue-13/viper.html

### “Event” Patterns

These are the idiomatic ways for components to notify others about things:

* __Delegation:__ _(one-to-one)_ Apple uses this a lot (some would say, too much). Use when you want to communicate stuff back e.g. from a modal view.
* __Callback blocks:__ _(one-to-one)_ Allow for a more loose coupling, while keeping related code sections close to each other. Also scales better than delegation when there are many senders.
* __Notification Center:__ _(one-to-many)_ Possibly the most common way for objects to emit “events” to multiple observers. Very loose coupling — notifications can even be observed globally without reference to the dispatching object.
* __Key-Value Observing (KVO):__ _(one-to-many)_ Does not require the observed object to explicitly “emit events” as long as it is _Key-Value Coding (KVC)_ compliant for the observed keys (properties). Usually not recommended due to its implicit nature and the cumbersome standard library API.
* __Signals:__ _(one-to-many)_ The centerpiece of [ReactiveCocoa][reactivecocoa-github], they allow chaining and combining to your heart's content, thereby offering a way out of [callback hell][elm-escape-from-callback-hell].

[elm-escape-from-callback-hell]: http://elm-lang.org/learn/Escape-from-Callback-Hell.elm

### Models

Keep your models immutable, and use them to translate the remote API's semantics and types to your app. Github's [Mantle](https://github.com/Mantle/Mantle) is a good choice.

### Views

When laying out your views using Auto Layout, be sure to add the following to your class:

    + (BOOL)requiresConstraintBasedLayout
    {
        return YES;
    }

Otherwise you may encounter strange bugs when the system doesn't call `-updateConstraints` as you would expect it to.

### Controllers

Use dependency injection, i.e. pass any required objects in as parameters, instead of keeping all state around in singletons. The latter is okay only if the state _really_ is global.

```objective-c
+ [[FooDetailsViewController alloc] initWithFoo:(Foo *)foo];
```

## Networking 网络

### Traditional way: Use custom callback blocks  传统的方式：使用回调 block

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

This works, but can quickly lead to callback hell if you need to chain multiple requests.

这个可以运行，但是如果你有多个组合的网络请求的时候就会进入回调嵌套的地狱。

### Reactive way: Use RAC signals Reactive 的方法： 使用 RAC 信号

If you find yourself in callback hell, have a look at [ReactiveCocoa (RAC)][reactivecocoa-github]. It's a versatile and multi-purpose library that can change the way people write [entire apps][groceryList-github], but you can also use it sparingly where it fits the task.

There are good introductions to the concept of RAC (and FRP in general) on [Teehan+Lax][teehan-lax-rac] and [NSHipster][nshipster-rac].

如果你发现已经陷入了回调的地狱，看看 [ReactiveCocoa (RAC)][reactivecocoa-github] 吧，它是一个万能而且多功能的库，能改变大家写  [entire apps][groceryList-github] 的方法，但是你可以在它适用的地方保守地使用它。

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

This allows us to transform or filter gigs before showing them, by combining the gig signal with other signals.

它允许在展示它们之前做一些改变，通过信号和其他信号的结合

## Assets 资源

[Asset catalogs][asset-catalogs] are the best way to manage all your project's visual assets. They can hold both universal and device-specific (iPhone 4-inch, iPhone Retina, iPad, etc.) assets and will automatically serve the correct ones for a given name. Teaching your designer(s) how to add and commit things there (Xcode has its own built-in Git client) can save a lot of time that would otherwise be spent copying stuff from emails or other channels to the codebase. It also allows them to instantly try out their changes and iterate if needed.

[Asset catalogs][asset-catalogs] 是管理你所有项目可视化资源的最好方式，它们可以同事管理通用的以及设备相关的iPhone 4-inch, iPhone Retina, iPad, etc.)资源，并且会自动通过它们的名字分组。告诉你的设计师如何增加它们（Xcode有内置的 Git 客户端）可以节省很多时间，否则你会话很多时间在从邮件或者其他渠道复制到代码库里面。它可以让设计师马上尝试资源改变的样子并且反复实验。

[asset-catalogs]: https://developer.apple.com/library/ios/recipes/xcode_help-image_catalog-1.0/Recipe.html

### Using Bitmap Images  使用位图

Asset catalogs expose only the names of image sets, abstracting away the actual file names within the set. This nicely prevents asset name conflicts, as files such as `button_large@2x.png` are now namespaced inside their image sets. However, some discipline when naming assets can make life easier:

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

The modifiers `-568h`, `@2x`, `~iphone` and `~ipad` are not required per se, but having them in the file name when dragging the file to an image set will automatically place them in the right "slot", thereby preventing assignment mistakes that can be hard to hunt down.

修饰名字 `-568h`, `@2x`, `~iphone` and `~ipad` 是不必要的，但是有他们在文件里面，当吧文件拖进去的时候，Xcode会正确地处置它们。这避免赋值错误。

### Using Vector Images 使用向量图

You can include the original [vector graphics (PDFs)][vector-assets] produced by designers into the asset catalogs, and have Xcode automatically generate the bitmaps from that. This reduces the complexity of your project (the number of files to manage.)

你可以用设计师原始的 [vector graphics (PDFs)][vector-assets] 加入到

[vector-assets]: http://martiancraft.com/blog/2014/09/vector-images-xcode6/ asset catalogs，Xcode可以自动地根据它们生成位图。这减少了你的工程的复杂性（管理更少的文件）。

## Coding Style 代码风格

### Naming 命名

Apple pays great attention to keeping naming consistent, if sometimes a bit verbose, throughout their APIs. When developing for Cocoa, you make it much easier for new people to join the project if you follow [Apple's naming conventions][cocoa-coding-guidelines].

尽管命名约定很长，但是Apple一如既往地在 API中 遵守了命名原则。

[cocoa-coding-guidelines]: https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html

Here are some basic takeaways you can start using right away:

这里有一些你应该使用的基本原则：

A method beginning with a _verb_ indicates that it performs some side effects, but won't return anything:

一个方法用 _动词_ 开头的表示它做了一些副作用，但是不会返回任何东西：
`- (void)loadView;`
`- (void)startAnimating;`

Any method starting with a _noun_, however, returns that object and should do so without side effects:
任何以 _名词_ 开头的方法，没有副作用而且返回了它指的对象：
`- (UINavigationItem *)navigationItem;`
`+ (UILabel *)labelWithText:(NSString *)text;`

It pays off to keep these two as separated as possible, i.e. not perform side effects when you transform data, and vice versa. That will keep your side effects contained to smaller sections of the code, which makes it more understandable and facilitates debugging.



### Structure 结构

[Pragma marks](http://nshipster.com/pragma/) are a great way to group your methods, especially in view controllers. Here is a common structure that works with almost any view controller:

[Pragma marks](http://nshipster.com/pragma/)  是把你代码分组的一个好方法，特别是在 view Controller里。这里有可以适用于绝大多数 view Controller的工作。

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

The most important point is to keep these consistent across your project's classes.

最重要的一点是在你项目的类中保持一致性

### External Style Guides 其他风格指南

Futurice does not have company-level guidelines for coding style. It can however be useful to peruse the Objective-C style guides of other development shops, even if some bits can be quite company-specific or opinionated:

Futurice 没有任何公司级别的代码风格指南。


* [GitHub](https://github.com/github/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [The New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Ray Wenderlich](https://github.com/raywenderlich/objective-c-style-guide)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [Luke Redpath](http://lukeredpath.co.uk/blog/2011/06/28/my-objective-c-style-guide/)

## Diagnostics 诊断

### Compiler warnings 编译警告

It is recommended that you enable as many compiler warnings as possible, and treat warnings as errors. This recommendation is justified in [these presentation slides][warnings-slides]. The slides also contain information on how to suppress certain warnings in specific files, or in specific sections of code.

推荐你尽可能多打开编译警告，并且像对待错误一样对待编译警告。推荐 [these presentation slides][warnings-slides]。这个幻灯片同时包括了如何在特定文件或者特别代码段里里面消除相关警告的内容。

In short, add at least these values to the _“Other Warning Flags”_ build setting:

简单的来说，至少下面的值需要在  _“Other Warning Flags” 编译设置里面：

- `-Wall` _(Enables lots of additional warnings)_
- `-Wextra` _(Enables more additional warnings)_

Also enable the _“Treat warnings as errors”_ build setting.

同时打开 _“Treat warnings as errors”_ 

[warnings-slides]: https://speakerdeck.com/hasseg/the-compiler-is-your-friend

### Clang Static Analyzer Clang 静态分析

The Clang compiler (which Xcode uses) has a _static analyzer_ that performs control and data flow analysis on your code and checks for lots of errors that the compiler cannot.

Clang 编译器（Xcode使用的）有一个 _静态分析器_ 来进行你的代码控制和数据流的分析，来检测编译器不能检测的许多错误。

You can manually run the analyzer from the _Product → Analyze_ menu item in Xcode.

你可以通过在 Xcode 里面手动运行  _Product → Analyze_ 菜单项来手动执行代码分析

The analyzer can work in either “shallow” or “deep” mode. The latter is much slower but may find more issues due to cross-function control and data flow analysis.

分析器可以用浅或者深的模式允许，后者更加慢但是可以从跨函数的控制流和数据流上分析更多问题

Recommendations:

推荐：

- Enable _all_ of the checks in the analyzer (by enabling all of the options in the “Static Analyzer” build setting sections)
- Enable the _“Analyze during ‘Build’”_ build setting for your release build configuration to have the analyzer run automatically during release builds. (Seriously, do this — you’re not going to remember to run it manually.)
- Set the _“Mode of Analysis for ‘Analyze’”_ build setting to _Shallow (faster)_
- Set the _“Mode of Analysis for ‘Build’”_ build setting to _Deep_

### [Faux Pas](http://fauxpasapp.com/)

Created by our very own [Ali Rantakari][ali-rantakari-twitter], Faux Pas is a fabulous static error detection tool. It analyzes your codebase and finds issues you had no idea even existed. Be sure to run this before shipping any iOS (or Mac) app!

_(Note: all Futurice employees get a free license to this — just ask Ali.)_

[ali-rantakari-twitter]: https://twitter.com/AliRantakari

### Debugging

When your app crashes, Xcode does not break into the debugger by default. To achieve this, add an exception breakpoint (click the "+" at the bottom of Xcode's Debug Navigator) to halt execution whenever an exception is raised. In many cases, you will then see the line of code responsible for the exception. This catches any exception, even handled ones. If Xcode keeps breaking on benign exceptions in third party libraries e.g., you might be able to mitigate this by choosing _Edit Breakpoint_ and setting the _Exception_ drop-down to _Objective-C_. 

For view debugging, [Reveal][reveal] and [Spark Inspector][spark-inspector] are two powerful visual inspectors that can save you hours of time, especially if you're using Auto Layout and want to locate views that are collapsed or off-screen. Granted, Xcode offers [something very similar][xcode-view-debugging] for free, but it's iOS 8+ only and feels somewhat less polished.

[reveal]: http://revealapp.com/
[spark-inspector]: http://sparkinspector.com
[xcode-view-debugging]: https://developer.apple.com/library/ios/recipes/xcode_help-debugger/using_view_debugger/using_view_debugger.html

### Profiling

Xcode comes with a profiling suite called Instruments. It contains a myriad of tools for profiling memory usage, CPU, network communications, graphics and much more. It's a complex beast, but one of its more straight-forward use cases is tracking down memory leaks with the Allocations instrument. Simply choose _Product_ > _Profile_ in Xcode, select the Allocations instrument, hit the Record button and filter the Allocation Summary on some useful string, like the prefix of your own app's class names. The count in the Persistent column then tells you how many instances of each object you have. Any class for which the instance count increases indiscriminately indicates a memory leak.

Also good to know is that Instruments has an Automation tool for recording and playing back UI interactions as JavaScript files. [UI Auto Monkey][ui-auto-monkey] is a script that will use Automation to randomly pummel your app with taps, swipes and rotations which can be useful for stress/soak testing.

[ui-auto-monkey]: https://github.com/jonathanpenn/ui-auto-monkey

## Analytics

Including some analytics framework in your app is strongly recommended, as it allows you to gain insights on how people actually use it. Does feature X add value? Is button Y too hard to find? To answer these, you can send events, timings and other measurable information to a service that aggregates and visualizes them – for instance, [Google Tag Manager][google-tag-manager]. The latter is more versatile than Google Analytics in that it inserts a data layer between app and Analytics, so that the data logic can be modified through a web service without having to update the app.

[google-tag-manager]: http://www.google.com/tagmanager/

A good practice is to create a slim helper class, e.g. `XYZAnalyticsHelper`, that handles the translation from app-internal models and data formats (XYZModel, NSTimeInterval, …) to the mostly string-based data layer:

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

This has the additional advantage of allowing you to swap out the entire Analytics framework behind the scenes if needed, without the rest of the app noticing.

### Crash Logs

First you should make your app send crash logs onto a server somewhere so that you can access them. You can implement this manually (using [PLCrashReporter][plcrashreporter] and your own backend) but it’s recommended that you use an existing service instead — for example one of the following:

* [Crashlytics](http://www.crashlytics.com)
* [HockeyApp](http://hockeyapp.net)
* [Crittercism](https://www.crittercism.com)
* [Splunk MINTexpress](https://mint.splunk.com)

[plcrashreporter]: https://www.plcrashreporter.org

Once you have this set up, ensure that you _save the Xcode archive (`.xcarchive`)_ of every build you release. The archive contains the built app binary and the debug symbols (`dSYM`) which you will need to symbolicate crash reports from that particular version of your app.


## Building

### Build Configurations

Even simple apps can be built in different ways. The most basic separation that Xcode gives you is that between _debug_ and _release_ builds. For the latter, there is a lot more optimization going on at compile time, at the expense of debugging possibilities. Apple suggests that you use the _debug_ build configuration for development, and create your App Store packages using the _release_ build configuration. This is codified in the default scheme (the dropdown next to the Play and Stop buttons in Xcode), which commands that _debug_ be used for Run and _release_ for Archive.

However, this is a bit too simple for real-world applications. You might – no, [_should!_][futurice-environments] – have different environments for testing, staging and other activities related to your service. Each might have its own base URL, log level, bundle identifier (so you can install them side-by-side), provisioning profile and so on. Therefore a simple debug/release distinction won't cut it. You can add more build configurations on the "Info" tab of your project settings in Xcode.

[futurice-environments]: https://blog.futurice.com/five-environments-you-cannot-develop-without

#### `xcconfig` files for build settings

Typically build settings are specified in the Xcode GUI, but you can also use _configuration settings files_ (“`.xcconfig` files”) for them. The benefits of using these are:

- You can add comments to explain things
- You can `#include` other build settings files, which helps you avoid repeating yourself:
    - If you have some settings that apply to all build configurations, add a `Common.xcconfig` and `#include` it in all the other files
    - If you e.g. want to have a “Debug” build configuration that enables compiler optimizations, you can just `#include "MyApp_Debug.xcconfig"` and override one of the settings
- Conflict resolution and merging becomes easier

Find more information about this topic in [these presentation slides][xcconfig-slides].

[xcconfig-slides]: https://speakerdeck.com/hasseg/xcode-configuration-files

### Targets

A target resides conceptually below the project level, i.e. a project can have several targets that may override its project settings. Roughly, each target corresponds to "an app" within the context of your codebase. For instance, you could have country-specific apps (built from the same codebase) for different countries' App Stores. Each of these will need development/staging/release builds, so it's better to handle those through build configurations, not targets. It's not uncommon at all for an app to only have a single target.

### Schemes

Schemes tell Xcode what should happen when you hit the Run, Test, Profile, Analyze or Archive action. Basically, they map each of these actions to a target and a build configuration. You can also pass launch arguments, such as the language the app should run in (handy for testing your localizations!) or set some diagnostic flags for debugging.

Schemes 告诉 Xcode 在你点击 Run, Test, Profile, Analyze or Archive 操作的时候应该怎么做。它们吧这些操作映射到一个 target 和一个构建设置中。你可以传递启动参数，比如 app 需要允许的语言（为了测试本地话）或者一些了为了调试用的诊断标志。

A suggested naming convention for schemes is `MyApp (<Language>) [Environment]`:

一个建议的 Scheme 命名是 `MyApp (<Language>) [Environment]`:

    MyApp (English) [Development]
    MyApp (German) [Development]
    MyApp [Testing]
    MyApp [Staging]
    MyApp [App Store]

For most environments the language is not needed, as the app will probably be installed through other means than Xcode, e.g. TestFlight, and the launch argument thus be ignored anyway. In that case, the device language should be set manually to test localization.


对于大多数环境来说语言是不必要的，app会可能会以非Xcode的方式安装，比如 TestFlight， 启动参数也会被忽略。这个情况下，为了测试本地话需要手动设置设备的语言。

## Deployment

Deploying software on iOS devices isn't exactly straightforward. That being said, here are some central concepts that, once understood, will help you tremendously with it.

### Signing 签名

Whenever you want to run software on an actual device (as opposed to the simulator), you will need to sign your build with a __certificate__ issued by Apple. Each certificate is linked to a private/public keypair, the private half of which resides in your Mac's Keychain. There are two types of certificates:

当你需要在真实设备上允许软件的时候，你需要用一个 Apple 认证的   __证书__ 签名。每一个证书是连接到一个 公、私 密钥对，私钥会保存在你的 Mac 的 KeyChain里面，证书有两种类型

* __Development certificate:__ Every developer on a team has their own, and it is generated upon request. Xcode might do this for you, but it's better not to press the magic "Fix issue" button and understand what is actually going on. This certificate is needed to deploy development builds to devices.
* __Distribution certificate:__ There can be several, but it's best to keep it to one per organization, and share its associated key through some internal channel. This certificate is needed to ship to the App Store, or your organization's internal "enterprise app store".


* __Development certificate:__ 每个组的开发者都有自己的证书，而且它通过请求特到。Xcode可以帮你完成，但是最好不用点击 "Fix issue" 来完成，而是理解它到底做了什么事情。在部署开发版本到设备上的时候需要这个证书。
* __Distribution certificate:__ 发布证书，可能有多重，但是最好每一个组织有一个，并且通过内部渠道共享。在提交 App 到 App Store 或者你的企业的内部 App Store的时候需要这个证书。

### Provisioning

Besides certificates, there are also __provisioning profiles__, which are basically the missing link between devices and certificates. Again, there are two types to distinguish between development and distribution purposes:

除了证书，还有  __provisioning profiles__， 它把设备和证书连接起来。而且，它分成 开发 和 分发两种累心。

* __Development provisioning profile:__ It contains a list of all devices that are authorized to install and run the software. It is also linked to one or more development certificates, one for each developer that is allowed to use the profile. The profile can be tied to a specific app, but for most development purposes it's perfectly fine to use the wildcard profile, whose App ID ends in an asterisk (*).


* __Development provisioning profile: 它包含了一个包含所有能安装这个 app 的设备列表。它同事连接了一个或者多个开发者允许这个 profile使用的证书。profile 可以确认特定的 app，但是对于大多数开发目的，它特别适合用一个通配符 profile，也就是 Apple ID 以一个 星号 (*)结尾的。

* __Distribution provisioning profile:__ There are three different ways of distribution, each for a different use case. Each distribution profile is linked to a distribution certificate, and will be invalid when the certificate expires.
    * __Ad-Hoc:__ Just like development profiles, it contains a whitelist of devices the app can be installed to. This type of profile can be used for beta testing on 100 devices per year. For a smoother experience and up to 1000 distinct users, you can use Apple's newly acquired [TestFlight][testflight] service. Supertop offers a good [summary of its advantages and issues][testflight-discussion].
    * __App Store:__ This profile has no list of allowed devices, as anyone can install it through Apple's official distribution channel. This profile is required for all App Store releases.
    * __Enterprise:__ Just like App Store, there is no device whitelist, and the app can be installed by anyone with access to the enterprise's internal "app store", which can be just a website with links. This profile is available only on Enterprise accounts.


* __Distribution provisioning profile:__ 发布 profile 本身，对于不同使用目的也有不同的类型。每一个发布 profile  链接到一个发布证书，并且在证书过期的时候失效。
    * __Ad-Hoc:__ 就像开发证书一个，它包含了一个 app 可以安装的设备的白名单。这个 profile 可以用来做 beta 版本，每年可以测试 100 个设备。为了做更细致的测试以及升级到 1000 个测试用户，你可以使用 Apple 最近发布的  [TestFlight][testflight] 服务， Supertop 提供了一个 [summary of its advantages and issues][testflight-discussion].
    * __App Store:__  这个 profile 没有列出设备，任何人都可以通过苹果官方的发布来安装，所有 App Store 发布的 App都需要这个证书
    * __Enterprise:__ 就像 App Store，没有设备白名单，任何通过企业内部网络的人都可以从内部“应用商店”安装。应用商店可能只是一个带连接的网站。这个 profile 只允许企业账号使用。




[testflight]: https://developer.apple.com/testflight/
[testflight-discussion]: http://blog.supertop.co/post/108759935377/app-developer-friends-try-testflight

To sync all certificates and profiles to your machine, go to Accounts in Xcode's Preferences, add your Apple ID if needed, and double-click your team name. There is a refresh button at the bottom, but sometimes you just need to restart Xcode to make everything show up.


想同步所有的证书和 profile到你的机器，可以去 Xcode 的  Preferences 的 Accounts下，添加你的 Apple ID, 然后双击你的 Team 名称。然后底部会有一个刷新按钮，有时候你需要重启 Xcode 来让东西显示出来。

#### Debugging Provisioning

Sometimes you need to debug a provisioning issue. For instance, Xcode may refuse to install the build to an attached device, because the latter is not on the (development or ad-hoc) profile's device list. In those cases, you can use Craig Hockenberry's excellent [Provisioning][provisioning] plugin by browsing to `~/Library/MobileDevice/Provisioning Profiles`, selecting a `.mobileprovision` file and hitting Space to launch Finder's Quick Look feature. It will show you a wealth of information such as devices, entitlements, certificates, and the App ID.

有时候你需要调试一个 provisioning 的问题。比如，XCode可能拒绝安装 app 到一个设备中，因为设备没有在 (development or ad-hoc)  的 profile 设备列表中。这个情况下，你可以使用 Craig Hockenberry 的优秀的 [Provisioning][provisioning] 插件，浏览 `~/Library/MobileDevice/Provisioning Profiles`，选择一个  `.mobileprovision` 文件，并用空格键调用 Finder 的快速查看特性，它会告诉你关于设备，entitlements，证书以及 Apple ID的信息。

[provisioning]: https://github.com/chockenberry/Provisioning

### Uploading 上传

[iTunes Connect][itunes-connect] is Apple's portal for managing your apps on the App Store. To upload a build, Xcode 6 requires an Apple ID that is part of the developer account used for signing. This can make things tricky when you are part of several developer accounts and want to upload their apps, as for mysterious reasons _any given Apple ID can only be associated with a single iTunes Connect account_. One workaround is to create a new Apple ID for each iTunes Connect account you need to be part of, and use Application Loader instead of Xcode to upload the builds. That effectively decouples the building and signing process from the upload of the resulting `.app` file.

[iTunes Connect][itunes-connect] 是苹果的管理你的上传到 App Store的 App 的后台。 Xcode 6 需要一个 Apple ID 作为开发者账号来签名并且上传二进制。This can make things tricky when you are part of several developer accounts and want to upload their apps, 因为 _一个 Apple ID 只能和一个 iTunes Connect 账号关联_, 一个变通方案是为每个 iTunes Connect 账号创建一个新的　Apple ID, 使用  Application Loader 取代 Xcode 来上传应用。这样有效地解除了上传最终 `.app` 文件构建和签名的耦合。

After uploading the build, be patient as it can take up to an hour for it to show up under the Builds section of your app version. When it appears, you can link it to the app version and submit your app for review.

再上传二进制后，耐心等待，可能要画上一个小时。当你的 App出现后，可以链接到对应的App版本并且提交审核

[itunes-connect]: https://itunesconnect.apple.com

## In-App Purchases (IAP) 应用内购买

When validating in-app purchase receipts, remember to perform the following checks:

当验证一个 App内购的  receipt的时候，记住做以下步骤：

- __Authenticity:__ That the receipt comes from Apple
- __Integrity:__ That the receipt has not been tampered with
- __App match:__ That the app bundle ID in the receipt matches your app’s bundle identifier
- __Product match:__ That the product ID in the receipt matches your expected product identifier
- __Freshness:__ That you haven’t seen the same receipt ID before.


- __Authenticity:__  receipt 是来自 Apple 的
- __Integrity:__  receipt 没有被篡改
- __App match:__ receipt 的 App bundle ID  和你的 App bundle ID 一致 
- __Product match:__ receipt 里面产品 ID和你期望的一致 
- __Freshness:__ 你之前没有验证一样的 receipt ID


Whenever possible, design your IAP system to store the content for sale server-side, and provide it to the client only in exchange for a valid receipt that passes all of the above checks. This kind of a design thwarts common piracy mechanisms, and — since the validation is performed on the server — allows you to use Apple’s HTTP receipt validation service instead of interpreting the receipt `PKCS #7` / `ASN.1` format yourself.

如果可能，把你的 IAP 存储销售相关的内容存储在服务器端，并且只在一个合法的经过上述检查的 receipt。这样的设计避免了常见的盗窃机制，同时，因为服务器做了验证，所以你可以使用 Apple 的 HTTP 验证服务来取代你自己的 `PKCS #7` / `ASN.1` 格式。

For more information on this topic, check out the [Futurice blog: Validating in-app purchases in your iOS app][futu-blog-iap].

更多关于这个主题的信息可以看[Futurice blog: Validating in-app purchases in your iOS app][futu-blog-iap]

[futu-blog-iap]: http://futurice.com/blog/validating-in-app-purchases-in-your-ios-app


## More Ideas  更多主题

- 3x assets, iPhone 6 screen sizes explained
- Add list of suggested compiler warnings
- Ask IT about automated Jenkins build machine
- Add section on Testing
- Add "proven don'ts"

- 3x 资源, iPhone 6 屏幕尺寸适配
- 增加 compiler warnings 列表
- 关于 Jenkins 的自动化测试
- 增加测试章节
- 增加“不要那么做”

[reactivecocoa-github]: https://github.com/ReactiveCocoa/ReactiveCocoa

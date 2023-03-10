# 2022 Android Dev Summit

## 1. 版本管理 Compose BOM
由于 Compose 组件太多，如果单独定义每个版本，容易导致出错，所以定义了一个 BOM 模块，用于声明一组内容库及其版本。之后只需要定一个 BOM 版本，就能够从中提取所有 Compose 相关的内容库版本。
每当 Compose 组件有新的稳定版本时，就回发布新的 BOM 版本，因此可以更加轻松的从一个稳定版转移到另一个稳定版
```Kotlin
dependencies {
    // 导入 Compose BOM
    implementation platform('androidx.compose2022.10.00')

    // 为尚未标明版本的目标 Compose 库声明依赖项
    implementation 'androidx.compose.foundation:foundation'
    androidTestImplementation 'androidx.compose.ui:ui-test-junit4'

    //implementation("androidx.compose.ui:ui:1.2.0-alpha08")
    //implementation("androidx.compose.material:material:1.2.0-alpha08")
    //implementation("androidx.compose.material3:material3:1.0.0-alpha12")
    //implementation("androidx.compose.material:material-icons-extended:1.2.0-alpha08@aar")
    //implementation("androidx.compose.ui:ui-tooling-preview:1.2.0-alpha08")
    implementation("androidx.compose.ui:ui")
    implementation("androidx.compose.material:material")
    implementation("androidx.compose.material3:material3")
    implementation("androidx.compose.ui:ui-tooling-preview")
    ...
}
```

## 2. 新增功能
### 使用全新 LazyHorizontalStaggeredGrid 和 LazyVerticalStaggeredGrid 实现交错网格
### 使用 DrawScope.drawText 直接在画布中绘制文本
### 使用 FontVariation 对象在应用中添加可变字体并更改其属性
### 在带注释的字符串中添加 UrlAnnotation 以改进与文本互动的无障碍服务
### 使用全新 LineBreakAPI 在您的文本中添加断字功能
### 使用全新 pullRefresh 修饰符下拉刷新
### 使用 SnapFlingBehavior 在您的惰性列表中添加贴靠行为
### LookAheadLayout 是一种新的布局类型，可以提供关于子项的最终测量与放置信息，帮助您决定中间层布局

## 3. Compose Material 3

## 4. Android Studio 工具
* Android Studio Dolphin 
    * 动画协调
    * MultiPreview 注释
    * 布局检查器中的重组计数

## 5. Relay
发布了 Relay 的首个 Alpha 版本，作为设计稿转代码的解决方案，可优化设计者与开发者之间的协作。设计者使用 Figma 插件创建界面组件，开发者则使用 Android Studio 插件将这些组件自动应用到他们的应用中。生成的组件是可组合函数，并可直接被集成到您的 Compose 应用中。您可以查看官方文档，详细了解 Relay。
* [Relay](https://relay.material.io/)
* [Figma 插件](https://www.figma.com/community/plugin/1041056822461507786)
* [Android Studio 插件](https://plugins.jetbrains.com/plugin/19721-relay-for-android-studio/)
* [官方文档](https://developer.android.google.cn/jetpack/compose/tooling/relay
)

## 6. 面向 Wear OS、大屏幕设备和电视的 Compose



## 7.性能优化
[Jetpack Compose 性能提升最佳实践](https://www.youtube.com/watch?v=EOQB8PTLkpY&list=PLWz5rJ2EKKc9Ty3Zl1hvMVUsXfkn93NRk)  
[深入了解 Jetpack Compose 布局](https://www.youtube.com/watch?v=zMKMwh9gZuI)
* 1. configuration: 配置文件，需要正式环境且打开R8。
```
buildTypes {
    release {
        minifyEnabled true
    }
}
```
* 2. remember: 
    * remember可以保证在数字变化的时候重组，而在重组的时候不会再次执行。
    * 键：lazylist key，需要保证key是唯一的
* 3. Deriving change: 衍生更改，主要是为了区分条件满足时才执行重组。比如滑动列表后显示"Move to top"按钮, listState的状态会一直刷新，我们只需要知道大于时显示按钮，但是继续滑动，不需要重组显示按钮
```
val showButton = remember {
    derivedStateOf {
        listState.firstVisibleItemIndex > 0
    }
}
```
* 4. Reading state: 延迟读取状态，例子是一个刷新操作。Compose三步骤（组合、布局、绘制）,能跳过的步骤尽量跳过
* 5. Running backwards: 在取值后又直接对该值进行了修改，导致系统判断当前值已废弃，执行了重组
* 6. Baseline Profiles
Compose is a library -- it does not participate in system resource sharing
Use Macrobenchmark to test the preformance benefit
[关于如何生产自己的配置文件以及如何配置Macrobenchmark](https://goo.gle/baseline-profiles)
[关于如何生产自己的配置文件以及如何配置Macrobenchmark](https://goo.gle/baseline-profiles-codelab)



----------------------------
----------------------------
----------------------------

# <font color="#499CD6">2022 Android 开发者峰会(2022 Android Dev Summit)</font>  
* 现代 Android 开发 (Modern Android Development)  
![Modern Android Development](./2022%20Android%20Dev%20Summit/dcim/01.modern%20android%20development.png "Modern Android Development")
* 设备类型 (Form Factors)  
![Form Factors](./2022%20Android%20Dev%20Summit/dcim/02.Form%20Factors.png "Form Factors")
* 平台 (Platform)  
![Platform](./2022%20Android%20Dev%20Summit/dcim/03.Platform.png "Platform")

## <font color="#499CD6">一、现代 Android 开发 (Modern Android Development)</font>  
---
Watch the deep technical sessions on your favorite Modern Android Development tools and APIs.  
观看您最喜爱的现代Android开发工具和API的深度技术会议。  
![现代Android开发](./2022%20Android%20Dev%20Summit/dcim/04.top3picks_01.png "现代Android开发")
1. <font color="#499CD6">Compose中的自定义布局和图形 (Custom Layouts and Graphics in Compose)</font>  
Jetpack Compose 提供了多种开箱即用的解决方案，可以快速轻松地从头开始构建屏幕。但是，当您需要更进一步并完全定制时会发生什么？在本次演讲中，您将学习如何使用自定义 Compose 布局和图形的强大组合来创建复杂的设计。我们将通过在短短 20 分钟内构建一个复杂的 Sleep Tracker 示例应用程序，通过更实际的方法介绍布局自定义图形、Compose 绘图操作和动画等内容。  
Jetpack Compose offers a variety of out-of-the-box solutions to quickly and easily build screens from scratch. But what happens when you need to go a step beyond and go fully custom? In this talk, you’ll learn how to create complex designs using a powerful combination of custom Compose Layouts and Graphics. We will cover things like laying out a custom graph, Compose drawing operations, and animations through a more hands on approach by building an intricate Sleep Tracker sample app in just 20 minutes.

1. <font color="#499CD6">Compose 修饰符深度教程 (Compose Modifiers Deep Dive)</font>  
深入了解修饰符的历史和 API 的限制。以及它们要解决的问题导致 1.3 中的实现进行重大改革，并添加了几个较低级别但功能强大的实验性 API，我们将在接下来的几个版本中迁移到这些 API。本次演讲将探讨此迁移的原因和方式、它如何影响开发人员以及它将对最终用户产生的性能影响。  
A deep dive into the history of Modifiers and the constraints of APIs. As well as the problems they are meant to solve leading up to a major overhaul in implementation in 1.3 and the addition of several lower level, but powerful, experimental APIs that we will be migrating to over the course of the next few releases. This talk will go into the why’s and how’s of this migration, how it impacts developers, and the performance impact it will have for end users.

1. <font color="#499CD6">界面层中的状态容器和状态生成 (State Holders and State Production in the UI Layer)</font>  
UI 层在屏幕上显示应用程序数据。但是具体是怎么做到的呢？在本次演讲中，我们将深入探讨管理 UI 复杂性的 UI 状态生产管道和状态持有者。此外，了解 UI 和业务逻辑、ViewModel 和普通状态持有者类、状态和事件等之间的区别！那是什么，什么时候使用哪个，以及如何使用。  
The UI layer displays application data on the screen. But how is it done exactly? In this talk, we'll dive deep into the UI state production pipeline and state holders that manage UI complexity. As well as, get to know the differences between UI and business logic, a ViewModel and a plain state holder class, state and events, and more! What is all that, when to use which, and how to do it.

1. <font color="#499CD6">利用基准配置文件大大加快应用运行速度 (Making Apps Blazing Fast with Baseline Profiles)</font>  
基线配置文件是一种显着提高应用程序和库的应用程序启动和运行时性能的新方法。在本次会议中，我们将分享如何创建基线配置文件并验证其有效性。当在各种 Android 平台版本上提供基线配置文件时，您还将发现 Android 运行时如何提高应用程序性能。  
Baseline Profiles are a new way to dramatically improve app startup and runtime performance of apps and libraries. In this session we will share how to create a Baseline Profile and verify its effectiveness. You will also discover how the Android Runtime improves app performance, when a Baseline Profile is provided on various Android platform versions.

1. <font color="#499CD6">最先进的 Compose 工具 (State of the Art of Compose Tooling)</font>  
在本次演讲中，我们将通过展示如何将这些工具整合到您的开发工作流程中，带您了解 Android Studio 中 Compose 工具的最新技术。  
您将学习如何使用 Compose Preview 设计和验证您的 UI，使用 Live Edit 加速您的开发工作流程，并使用 Compose 编辑功能更快地编写代码。我们还将向您展示如何分析您的布局，使用布局检查器了解重组，以及查明代码中的性能问题。  
在本次演讲之后，您将准备好利用这些工具来构建美观、高性能和自适应的 Compose UI。  
In this talk, we will walk you through the state of art of Compose Tooling in Android Studio by showing how to incorporate these tools into your development workflow.  
You’ll learn to design and validate your UI with Compose Preview, accelerate your development workflow with Live Edit, and write code faster with Compose editing features. We’ll also show you how to analyze your layout, understand recompositions with the Layout Inspector, and pinpoint performance issues in your code.  
After this talk, you will be ready to leverage these tools to build beautiful, performant, and adaptive Compose UI.

1. <font color="#499CD6">Android Build 的新变化 (What's New in Android Build)</font>  
在本次演讲中，我们想分享 Android Gradle 插件 (AGP) 中的新功能，以及新的 API 和功能如何帮助您提高生产力（维护和速度）。  
In this talk, we would like to share what is new in Android Gradle Plugin (AGP), and how the new APIs and features can help you with Build productivity (maintenance and speed).

1. <font color="#499CD6">从 Views 到 Compose：我该从何处入手? (From Views to Compose: Where Can I Start?)</font>  
使用 Jetpack Compose 并不意味着您需要从头开始重建您的应用程序，相反，您可以采用增量方法进行迁移。在本次演讲中，您将学习如何开始将 Compose 引入您的代码库，以及如何逐步迁移现有屏幕。在本次演讲之后，您将对如何将应用程序转换为 Compose 打下坚实的基础。  
Using Jetpack Compose doesn’t mean you need to rebuild your app from scratch, instead, you can take an incremental approach to migrating. In this talk, you will learn how to start introducing Compose into your codebase and how to gradually migrate existing screens. After this talk, you’ll have a solid foundation on how to approach converting your app to Compose.

1. <font color="#499CD6">Compose中状态提升的位置 (Where to Hoist that State in Compose?</font>  
在本次演讲中，您将了解如何以及在何处提升 Jetpack Compose 中的状态。什么时候应该挂起状态？它应该在可组合函数、普通状态持有者类中还是在 ViewModel 中？在本次会议中，我们将使用真实示例探索不同的可能性。
In this talk, you'll learn how and where to hoist state in Jetpack Compose. When should state be hoisted? Should that be in a composable function, plain state holder class, or in a ViewModel? In this session, we'll explore the different possibilities using real-world examples.

1. <font color="#499CD6">在 Compose 中使用 Material You (Material You in Compose Apps)</font>  
Material 3 Jetpack Compose 库将在 ADS 稳定运行！了解新的和更新的主题和组件，并开始在您的生产应用程序中使用该库。本次演讲还涵盖了 Material You 动态颜色和从 Material 2 迁移。来看看为什么现在采用 Jetpack Compose 让应用程序感觉新鲜，并有助于与 Android 操作系统视觉语言和美学的发展保持同步。  
The Material 3 Jetpack Compose library will be stable at ADS! Learn about new and updated theming and components, and get started using the library in your production apps. This talk also covers Material You dynamic color and migrating from Material 2. Come see why adopting Jetpack Compose now makes apps feel fresh and helps stay in sync with the evolution of the Android OS visual language and aesthetic.

1. <font color="#499CD6">Compose 改进页面测试的5中方式 (5 Ways Compose Improves UI Testing)</font>  
如果您需要另一个借口将您的应用程序迁移到 Compose，那么测试可组合项比测试视图更容易、更快和更可靠。在本次演讲中，我们将探讨由于 Compose 的设计方式而改进测试的五种方式。  
If you need another excuse to migrate your app to Compose, testing composables is easier, faster and more reliable than testing Views. In this talk, we'll look at five ways testing has improved thanks to how Compose was designed.

1. <font color="#499CD6">关于 Navigation 的类型安全的多模块最佳实践 (Type Safe, Multi-Module Best Practices with Navigation Compose)</font>  
随着您的应用程序的大小和复杂性的增加，遵循这些使用 Navigation Compose 的最佳实践将使您能够跨多个模块扩展导航图，从而在所有导航调用中保持类型安全。本次演讲还将解释如何将 Kotlin Multiplatform 就绪屏幕与导航代码分开，以及如何在将导航代码拆分到多个模块后将其重新组合在一起。  
As your app grows in size and complexity, following these best practices for using Navigation Compose will set you up for expanding your navigation graph across multiple modules in a way that maintains type safety across all navigation calls. This talk will also explain how to separate Kotlin Multiplatform ready screens from Navigation code and how to bring your Navigation code back together after splitting it across multiple modules.

1. <font color="#499CD6">Room 迁移实践 (Practical Room Migrations)</font>  
数据库迁移有时看起来像是一项极限运动 - 如果您同意，这就是适合您的话题！在本次演讲中，我们将介绍自动迁移、如何迁移预填充的数据库、如何为迁移预处理和后处理数据，以及如何在迁移期间处理外键和视图。有了这些新技能，迁移将不再像带着降落伞跳伞 - 但也许会感觉像带着喷气背包跳伞！  
Database migrations may seem like an extreme sport at times - if you agree, this is the talk for you! In this talk, we will cover auto migrations, how to migrate a pre-populated database, how to pre and post process data for migrations, and how to handle foreign keys & views during a migration. With these new skills, migrations won’t feel like skydiving with a parachute anymore - but will perhaps, feel like skydiving with a Jetpack!

1. <font color="#499CD6">利用 Gradle 管理的设备进行大规模测试 (Test at Scale with Gradle Managed Devices)</font>  
Gradle Managed Devices (GMD) 通过内置的测试缓存、分片和生命周期管理，可以轻松地利用虚拟设备进行可扩展、完全托管的测试。我们现在添加了对在 Firebase 测试实验室中运行的物理和虚拟设备的支持将 GMD 的优势带入 Google 的 Android 云测试解决方案。  
Gradle Managed Devices (GMD) makes it easy to leverage virtual devices for scalable, fully-managed testing, with test caching, sharding, and lifecycle management built in. We’re now adding support for both physical and virtual devices running in Firebase Test Lab to bring the benefits of GMDs to Google’s cloud testing solution for Android.

1. <font color="#499CD6">不容错过的 5 项 Android Studio 功能(5 Android Studio Features You Don't Want to Miss)</font>  
到目前为止，每个人都可能已经看到了 Jetpack Compose 工具、Live Edit 和 Android Studio 的其他重要功能的实际应用。这就是为什么在本次演讲中，我们将向您展示 IDE 中即将推出的 5 个功能和改进，这些功能和改进可能不太容易被注意到，但有机会极大地改进您的日常开发工作流程。  
By now everyone has probably seen Jetpack Compose tools, Live Edit, and other high profile features of Android Studio in action. That's why in this talk we'll show you 5 upcoming features and improvements in the IDE that are perhaps not as easily noticed, but have a chance to greatly improve your everyday development workflows.

1. <font color="#499CD6">Jetpack Compose 更多的性能提示 (More Performance Tips for Jetpack Compose)</font>  
Jetpack Compose 谈话中常见性能陷阱 I/O 的跟进。我们将进一步详细了解延迟读取 Compose 状态的原因，了解稳定性以及 Compose 如何推断稳定性，查看 reportFullyDrawn 的新 API，等等。  
A follow up to the Common Performance Gotchas I/O in Jetpack Compose talk. We will go further into the details of why deferring reads of Compose state works, learn about stability and how Compose infers it, have a look at a new API for reportFullyDrawn, and more.

1. <font color="#499CD6">从头开始构建可扩展、模块化、可测试的应用程序 (Building a Scalable, Modularized, Testable App From Scratch)</font>  
如果您正在从头构建应用程序或希望更新您的应用程序以遵循现代 Android 开发最佳实践，本次演讲将为您提供所需的所有部分的高级概述，以及它们如何使用真实世界组合在一起示例：Android 应用中的 Now。  
本次演讲还将解释我们如何构建应用程序的一项功能及其设计背后的决策。我们将介绍该应用程序的可测试模块化架构，并讨论我们如何使用 Jetpack Compose 和 Material3 构建一组可重复使用的 UI 元素。  
If you're building an app from scratch or looking to update your app to follow modern Android development best practices, this talk will give you a high-level overview of all the pieces you need, and how they fit together using a real-world example: the Now in Android app.  
This talk will also explain how we built one of the app's features and the decisions behind its design. We'll cover the app's testable, modular architecture and talk about how we built a set of reusable UI elements using Jetpack Compose and Material3.

1. <font color="#499CD6">重新规划 设计者-开发者 之间的协作：介绍 Relay (Reimagining Designer-Developer Handoff: Introducing Relay)</font>  
在本次闪电演讲中，我们将向您介绍 Relay，现已推出开放 alpha 版。Relay 是一种新流程，允许团队在 Figma 中创建 UI 并生成高保真 Compose UI 组件。Relay 将结构化组件数据置于设计人员和开发人员协作的中心，从而实现即时 UI 实现和快速迭代。  
In this lightning talk we’ll introduce you to Relay, available now in open alpha. Relay is a new process that allows teams to create UI in Figma and generate high-fidelity Compose UI components. Relay puts structured component data at the center of the collaboration between designer and developer, resulting in instant UI implementation and rapid iteration.

1. <font color="#499CD6">5 个快速动画让你的 Compose 应用脱颖而出 (5 Quick Animations to Make Your Compose App Stand Out)</font>  
想要为您的 Jetpack Compose 应用程序添加一些动作，但没有时间学习有关动画的所有知识？这里有 5 个快速动画，可让您的应用程序在短短几分钟内栩栩如生！  
Want to add some movement to your Jetpack Compose app, but don’t have the time to learn everything there is to know about Animations? Here are 5 quick animations to make your app come to life in just a few minutes!

1. <font color="#499CD6">Compose 文本样式 (Styling Text in Compose)</font>  
文本样式可以赋予您的应用个性。在本次演讲中，我们将使用 Jetchat 学习如何使用 Material API 来配置排版，包括使用可下载字体和可变字体。然后，我们将自定义我们的聊天气泡，使其根据消息的长度崩溃。我们将通过设置消息框的样式来结束：给它一个渐变边框、一个在您键入时改变颜色的光标和一个完全自定义的装饰框。  
Text styling can give your app character. In this talk, we’ll use Jetchat to learn how to use Material APIs to configure typography, including using downloadable fonts and variable fonts. We’ll then customize our chat bubble so it collapses depending on how long the message is. We’ll end by styling the message box: giving it a gradient border, a cursor that changes color as you type, and a fully custom decorated box.

1. <font color="#499CD6">创建离线首个应用程序 (Create Offline-First Apps)</font>  
没有网络？没问题！了解如何构建离线优先应用。本次演讲将涵盖建模、数据访问语义、同步和冲突解决。它还将重点介绍在构建离线应用程序时不可或缺的实用程序和数据结构。  
No network? No problem! Learn how to build an offline-first app. This talk will cover modeling, data access semantics, synchronization, and conflict resolution. It will also highlight the utilities and data structures that are indispensable when building offline first apps.

1. <font color="#499CD6">Android 应用程序模块化指南 (By Layer or Feature? Why Not Both?! Guide to Android App Modularization)</font>  
这个实用的演讲将为您提供一组通用模式和方法，用于在现代 Android 应用程序架构的上下文中模块化您的项目。了解模块的类型及其在多模块代码库中的作用。  
This practical talk will give you a set of common patterns and recipes for modularizing your project in the context of the modern Android app architecture. Learn the types of modules and their role in a multi-module codebase.

1. <font color="#499CD6">以生命周期感知的方式收集流 (Collecting Flows in a Lifecycle-Aware Manner)</font>  
以生命周期感知方式收集流是在 Android 上收集流的推荐方法。在本次演讲中，我们将探索您必须执行此操作的不同 API，例如 Jetpack Compose 中的 repeatOnLifecycle API 或 collectAsStateWithLifecycle API，并了解它们的工作原理。  
Collecting flows in a lifecycle-aware manner is the recommended way to collect flows on Android. In this talk, we'll explore the different APIs you have to do so, such as the repeatOnLifecycle API or collectAsStateWithLifecycle API in Jetpack Compose, and see how they work under the hood.

1. <font color="#499CD6">使用可评测构建准确测量应用程序性能 (Accurately Measure App Performance with Profileable Builds)</font>  
在本地开发期间，大多数应用程序开发人员在可调试模式下构建和运行他们的应用程序。然而，可调试的应用程序会导致显着和不同的性能下降，并且对准确测量时序没有用。在本次演讲中，了解可配置应用程序对性能测量的好处以及如何在 Android Studio 中构建它们。  
During local development, most app developers build and run their app in debuggable mode. However, debuggable apps incur significant and varied performance degradation and are not useful for measuring timing accurately. In this talk, learn the benefits of profileable apps for performance measurement and how to build them in Android Studio.

1. <font color="#499CD6">实现首个 Compose UI 测试 (Write Your First Compose UI Test)</font>  
在本次演讲中，我们将引导您编写您的第一个 Compose UI 测试。我们将介绍查找器、断言、操作和匹配器，并快速浏览一下语义树。  
In this talk we’ll walk you through writing your very first Compose UI test. We’ll cover finders, assertions, actions, and matchers and take a quick look at the semantics tree.

1. <font color="#499CD6">从 Android Studio 更快地解决 Firebase Crashlytics 报告 (Address Firebase Crashlytics Reports Faster from Android Studio)</font>  
Firebase Crashlytics 记录了开发人员的生产应用程序中发生的错误，但到目前为止，您需要转到 Crashlytics 的 Web 控制台来调查这些错误。Android Studio Electric Eel 中引入的 App Quality Insights 可以将错误与 Android Studio 集成，让您可以导航到导致错误的相关代码。  
本次演讲将解释 App Quality Insights 的基础知识，以及它如何有助于调试生产应用程序中的错误。  
Firebase Crashlytics records the errors that happen in developers' production apps, but up until now you needed to go to Crashlytics’s web console to investigate the errors. App Quality Insights, introduced in Android Studio Electric Eel, makes it available to integrate the errors with Android Studio, allowing you to navigate to the relevant code that causes the errors.  
This talk will explain the basics of App Quality Insights and how that can be useful in debugging errors in production apps.

## <font color="#499CD6">二、设备类型 (Form Factors)</font>   
---
Watch the videos to learn about the latest updates on building for different form factors and screens.  
观看视频，了解不同形状和屏幕的建筑最新更新。  
![设备类型](./2022%20Android%20Dev%20Summit/dcim/04.top3picks_02.png "设备类型")

1. <font color="#499CD6">利用 Android Studio 为各种类型设备打造更出色的界面 (Build Better UIs Across Form Factors with Android Studio)</font>  
Android Studio 使您可以更轻松、更快速地跨多种外形规格（从小型到大型）扩展您的应用程序！来参观一下 IDE，我们将带您了解新工具和改进的功能，例如 Visual Linting、Reference Devices、Resizeable and Wear Emulators、Wear Pairing Assistant、Form Factor Previews 等等！在本次演讲之后，您将准备好使用 Studio 的多设备环境加速您的工作流程，以构建大屏幕和 Wear OS。  
Android Studio is making it easier and faster to expand your app across multiple form factors--from small to large! Come take a tour across the IDE where we will walk you through new tools and improved features such as Visual Linting, Reference Devices, Resizeable and Wear Emulators, Wear Pairing Assistant, Form Factor Previews, and many more! After this talk, you will be ready to accelerate your workflow with Studio’s multi-device environment to build for Large Screens and Wear OS.

1. <font color="#499CD6">Compose：实现大屏幕的响应式 UI (Compose: Implementing Responsive UI for Large Screens)</font>  
了解如何为每种屏幕尺寸构建自适应布局。您将培养使用 Compose 构建 UI 的思维方式，以在手机、平板电脑、可折叠设备和 ChromeOS 设备上创造出色的用户体验。  
Learn how to build adaptive layouts for every screen size. You’ll develop the mindsets for building UI with Compose to create a great user experience across phones, tablets, foldables and ChromeOS devices.

1. <font color="#499CD6">优化大屏幕应用程序的思维倾向 (Do’s and Don’ts: Mindset for Optimizing Apps for Large Screens)</font>  
快来学习构建 Android 应用程序的最佳实践，以便它在更大的屏幕和可折叠设备上运行良好！我们将涵盖从新的 Android Studio 工具、新的和更新的 Jetpack 库到更具体的设计和开发指南的所有内容，使您可以轻松地利用超过 270M 的活动大屏幕 Android 设备！  
Come learn best practices for building your Android application so it will work well on larger screens and foldables! We'll cover everything from new Android Studio tools, new and updated Jetpack libraries, and more specific design and development guidance to make it easy for you to capitalize on the more than 270M active large screen Android devices!

1. <font color="#499CD6">设计适用于大屏设备的产品：规范布局和视觉层次结构 (Designing for Large Screens: Canonical Layouts and Visual Hierarchy)</font>  
规范布局为差异化的大屏幕体验提供了一个很好的起点，涵盖了常见的用例和屏幕尺寸。但是，您如何为您的应用程序选择正确的布局，或者在规范布局之上构建以创建与您的产品完美匹配的自适应体验？学习从设计角度和核心开发概念理解规范布局，解开提要、列表详细信息和支持面板布局的基本原理，并释放提升自适应设计水平的潜力。  
Canonical layouts provide a great starting point for differentiated large screen experiences, covering common use-cases and screen sizes. But how do you choose the right layout for your app, or build on top of canonical layouts to create an adaptive experience that’s perfectly matched to your product? Learn to understand canonical layouts from a design perspective and the core development concepts, unpacking the rationale of feeds, list-detail, and supporting panel layouts and unlocking the potential to level up your adaptive design.

1. <font color="#499CD6">在 Wear OS 上构建媒体应用 (Building Media Apps on Wear OS)</font>  
在本次演讲中，您将了解如何在 Wear OS 上构建高质量的媒体应用。我们将首先通过媒体应用程序的核心用户旅程来概述要构建的内容；然后，我们将讨论如何通过采用我们新发布的 Media Toolkit 和 Media3 Exoplayer 来简化开发，我们将以一些提示和技巧作为结尾，以确保良好的性能。  
In this talk you will learn how to build a high quality Media app on Wear OS. We will first go through the Core User Journeys for media apps to outline what to build; we will then discuss how to ease the development by adopting our newly released Media Toolkit and Media3 Exoplayer, and we will conclude with some tips and tricks to ensure good performances.

1. <font color="#499CD6">深入了解 Wear OS 应用架构 (Deep Dive into Wear OS App Architecture)</font>  
为 Wear OS 构建并不意味着从头开始学习 Android。本次演讲将教您如何将新的 Wear 项目添加到现有的移动项目，或者如何从头开始创建和构建 Wear 应用程序。我们将了解如何最好地组织您的代码以尽可能多地重复使用，并了解如何利用 Horologist 等工具为您的用户提供可靠的体验。  
Building for Wear OS doesn’t mean learning Android from scratch. This talk will teach you how to add a new Wear project to an existing mobile project, or how to create and structure a Wear app from scratch. We’ll see how best to organize your code to reuse as much as possible, and understand how to leverage tools like Horologist to provide a solid experience for your users.

1. <font color="#499CD6">利用健康服务和 Health Connect 打造实用的健身体验 (Creating Helpful Fitness Experiences with Health Services and Health Connect)</font>  
现代健康和健身体验存在于多种形式因素中。数据很少会在单个可穿戴设备、手机应用程序或设备上生死存亡。碰巧的是，包括智能手机和可穿戴设备在内的大量设备以及许多健康、健身和健康应用程序都在 Android 上运行。  
在本次演讲中，您将学习如何构建连贯的、周到的体验，将 Health Services 和 Health Connect 联系起来，并使用户能够控制他们的数据和隐私。  
Modern health and fitness experiences exist across multiple form factors. Rarely does data live and die on a single wearable, phone app, or piece of equipment. And it just so happens that a large portfolio of devices, including smartphones and wearables, and many health, fitness, and wellness apps run on Android.  
In this talk, you’ll learn how to build cohesive, thoughtful experiences that bridge Health Services and Health Connect and empower users to have control over their data and privacy.

1. <font color="#499CD6">改善电视用户体验 (Improving the TV User Experience)</font>   
最新的电视平台更新提供了一些很棒的新方法，可以为客厅中的应用程序提供更好的用户体验。  
The latest platform updates for TV offer some great new ways to deliver better user experiences to apps in the living room.

1. <font color="#499CD6">车载应用库都有哪些新的内容 (What’s New with the Car App Library)</font>  
了解最近添加到 Car App Library 中的新功能，这些新功能可以让 Android Auto 和 Android Automotive OS 上的驾驶优化应用比以往任何时候都更好！  
Learn about the new features that have recently been added to the Car App Library to make driving optimized apps better than ever on both Android Auto and Android Automotive OS!

1. <font color="#499CD6">通过多窗口和活动嵌入实现更多功能 (Do More with Multi-Window and Activity Embedding)</font>  
我们曾经认为用户会在任何给定时刻看到一个活动并与之交互。从 Android 12L 开始，该假设不再有效，因为 Android 12L+ 带来了多任务处理的前沿和中心，允许用户从不同的应用程序或同一应用程序在屏幕上进行两项活动。本次演讲将介绍您需要做些什么来确保您的应用程序可以在多窗口中启动，以及如何利用额外的空间并同时显示多个活动。  
We used to think that users will see and interact with one activity at any given moment. Starting with Android 12L that assumption is not valid any more as Android 12L+ brings multi-tasking front and center allowing the users to have two activities on the screen either from different apps or the same app. This talk will cover what you need to do in order to make sure your app can be launched in multi-window and how to take advantage of the extra real estate and show more than one activity at the same time.

1. <font color="#499CD6">不同设备类型上的相机应用 (Your Camera App on Different Form Factors)</font>  
从历史上看，您的应用程序可能在其整个生命周期中都存在于同一个窗口中并且具有固定的方向。但是随着可折叠设备等新外形因素的出现，以及多窗口和多显示器等新显示模式的出现，您不能再认为这会是真的了。让我们看看在开发针对大屏幕和可折叠设备的应用时需要考虑的一些最重要的因素。  
Historically, your app could have lived in the same window and with a fixed orientation for its whole life cycle. But with the availability of new form factors, such as foldable devices, and new display modes such as multi-window and multi-display, you can't assume this will be true anymore. Let's see some of the most important considerations when developing an app targeting large screen and foldable devices.

1. <font color="#499CD6">Compose 导航在不同屏幕大小下的体现 (Navigation Compose on Every Screen Size)</font>  
编写一个可以处理电话、ChromeOS 设备以及介于两者之间的所有内容的单一导航系统可能会让人望而生畏。我们将讨论完成这项工作的策略，以及如何将 Navigation Compose 与规范布局一起使用，为大屏幕打造最佳体验，无缝适应手机屏幕。  
Writing a single navigation system that can handle phones, ChromeOS devices, and everything in between can appear daunting. We’ll talk through strategies for approaching this work and how Navigation Compose can be used alongside the canonical layouts to build the best experience for large screens that seamlessly adapts down to phone screens.

1. <font color="#499CD6">插件：Compose 版本管理 (Compose BOM) (Insets: Compose Edition)</font>  
不要害怕边到边！了解 insets 如何与放置系统装饰的应用程序通信，以及新的 Compose API 如何帮助您的内容自动随系统栏、软件键盘和任务栏移动。  
Don’t be afraid to go edge-to-edge! Learn how insets communicate to your app where system decorations are placed, and how the new Compose APIs help your content automatically move with the system bars, software keyboard, and the taskbar.

1. <font color="#499CD6">在平板电脑和 ChromeOS 设备上实现键盘和鼠标支持的关键 (The Key to Keyboard and Mouse Support across Tablets and ChromeOS)</font>  
Android 目前拥有超过 2.7 亿台活跃的大屏幕设备。随着每一种新的大屏幕设备的推出，优化您的应用程序以支持键盘和鼠标的重要性不断增加。本次演讲深入探讨了可用于在您的应用程序中引入和优化键盘和鼠标支持的代码。  
Android has over 270 million active large screen devices today. With every new large screen device introduced, the importance of optimizing your app for keyboard and mouse support continues to grow. This talk dives into the code you can use to introduce and optimize keyboard and mouse support in your app.

1. <font color="#499CD6">跨设备开发语音助手 (Developing for Assistant across Devices)</font>    
在本次演讲中，您将了解如何利用 Android Studio 中的语音优先 API 和工具，通过 Google Assistant 将语音功能引入适用于各种设备类型的应用。  
In this talk, you'll learn how to take advantage of voice-first APIs and tooling in Android Studio to bring voice functionality through Google Assistant to your apps for various device types.

1. <font color="#499CD6">Google Play 在三种不同类型大屏上的效果 (Three Tiers of Large Screen Quality on Google Play)</font>  
平板电脑和可折叠设备的日益普及为以创新方式满足新用户群提供了机会。响应式 UI 可让您轻松打造这种体验。  
在本次演讲中，您将了解开发人员可以使用哪些工具来支持大屏幕以在 Android 上创建和测试响应式 UI，以便用户无论在什么设备上使用您的应用程序都会喜欢它。  
The rising popularity of tablets and foldable devices unlocks opportunities to address a new range of users in innovative ways. A responsive UI allows you to easily build this experience.  
In this talk, you’ll get an understanding of what’s available for developers to support large screens to create and test responsive UIs on Android so that users love your app no matter what device they’re using it on.

1. <font color="#499CD6">拖放实现无缝多任务 (Drag & Drop for Seamless Multitasking)</font>  
随着大屏设备的增多，越来越多的用户同时使用多个应用。通过添加对从您的应用程序拖放内容或将内容拖放到应用程序中的支持，您可以减少摩擦并通过出色的跨应用程序交互取悦您的用户！  
With the increase in large screen devices, users are using multiple apps at the same time more and more. By adding support for dragging and dropping content from/into your app, you can cut out friction and delight your users with great cross-app interactions!

1. <font color="#499CD6">针对 ChromeOS 优化的理由和方式 (Why and How to Optimize Your App for ChromeOS)</font>  
今天，ChromeOS 上有数百万个 Android 应用程序可用，如果您的应用程序在 Google Play 中，它可能就是其中之一。本次演讲将概述我们如何采取措施确保我们的用户在 ChromeOS 上获得最佳体验。  
Millions of Android apps are available on ChromeOS today, and, if your app is in Google Play, it may be one of them. This talk will provide an overview of how we can take steps to ensure the best possible experience for our users on ChromeOS.

1. <font color="#499CD6">为Android应用程序添加触控笔支持 (Adding Stylus Support to Your Android App)</font>  
了解如何针对手写笔输入优化 Android 应用。通过利用新的 Jetpack 库，您可以使用手写笔设备引入身临其境的用户体验，以实现与在纸上书写类似的体验。  
Learn how to optimize your Android apps for stylus input. By leveraging new Jetpack libraries, you can introduce immersive user experiences with stylus devices to achieve a similar experience to putting pen to paper.

1. <font color="#499CD6">不费吹灰之力测试Wear OS健身应用程序 (Testing Wear OS Fitness Apps Without Breaking a Sweat)</font>  
与手机应用程序相比，为 Wear 开发高质量的健康和健身体验——尤其是执行手动 QA——可能有点棘手。功能因设备而异，一遍又一遍地跑步（或游泳！）来测试用户旅程是不切实际的。在本次灯光演讲中，我们通过模拟器和健康服务的合成模式，讨论了一些用于测试双脚牢牢踩在地上的健康和健身体验的选项。  
Compared to phone apps, developing high quality health and fitness experience for Wear—and especially performing manual QA—can be a bit tricky. Capabilities vary between devices, and running (or swimming!) over and over to test a user journey is not practical. In this lighting talk we go over a few options for testing health and fitness experiences with two feet firmly on the ground, via the emulator and health services' synthetic mode.

1. <font color="#499CD6">围绕手表：在 Wear OS 中处理旋转输入 (Around a Watch: Handling Rotary Input in Wear OS)</font>  
Wear OS 设备可能包含物理表冠或旋转表圈。当用户转动表冠时，系统会生成旋转事件，开发人员可以利用这些事件为用户提供增强的触觉交互。例如，这可用于滚动屏幕或控制音量。在本次演讲中，您将学习如何在您的应用中处理旋转输入。  
Wear OS devices may contain a physical crown or a rotating bezel. When a user turns the crown, the system generates rotary events that developers can utilize to provide enhanced tactile interactions to the user. For example, this can be used for scrolling screens or to control audio volume. In this talk you will learn how to handle rotary input in your app.

1. <font color="#499CD6">让您的应用程序为Google Play上的所有设备闪耀 (Make Your App Shine for all Devices on Google Play)</font>  
您在 Google Play 上的应用列表信息即将获得更多播放时间！了解商店中的新功能以及在这些变化之前优化您的应用资产的最佳实践。  
Your app listing information on Google Play is about to get more air time! Learn about new features in store and best practices for optimizing your app assets ahead of these changes.

## <font color="#499CD6">平台 (Platform)</font>  
---
Tune in to learn about the latest updates to the Android platform.  
请关注以了解Android平台的最新更新。  
![平台](./2022%20Android%20Dev%20Summit/dcim/04.top3picks_03.png "平台")

1. <font color="#499CD6">将你的应用迁移到 Android 13 (Migrate Your Apps to Android 13)</font>  
每个新版本的 Android 都会带来您的应用需要考虑的平台行为变化；其中一些更改仅在您针对新的 SDK 版本时适用，而其他更改（主要涉及隐私和安全）适用于所有应用程序。我们将介绍这些变化，深入了解如何测试您的应用，并讨论您可以利用的新 Android 13 功能，为您的 Android 13 早期采用者提供最佳体验。  
Each new release of Android comes with platform behavior changes that your app needs to account for; some of these changes apply only when you target the new SDK version, while others — mostly involving privacy and security — apply to all apps. We'll cover these changes, give insight on how to test your app, and talk about new Android 13 features you can take advantage of to give your Android 13 early adopters the best experience.

1. <font color="#499CD6">为所有的用户提供高质量的媒体体验 (Presenting a High-quality Media Experience for all Users)</font>    
媒体体验强烈依赖于多种因素，例如设备的硬件功能和媒体文件本身的属性，形成了开发人员需要处理的复杂场景矩阵。本次演讲将讨论用于确保您的媒体应用程序经过优化以向所有用户呈现最佳体验的工具和策略，无论使用情况如何。  
Media experiences have a strong dependence on a variety of factors, like the device’s hardware capabilities and the properties of the media file itself, forming a complicated matrix of scenarios that developers need to handle. This talk will discuss tools and strategies for making sure your media app is optimized to present the best experience to all your users regardless of the use-case.

1. <font color="#499CD6">使用 Android 相机提高你的社交体验质量 (Improving Your Social Experience Quality with Android Camera)</font>  
在本次会议中，我们将探索新的框架创新，以提高质量、改善延迟并使用 Android Camera 打造创新体验。  
In this session, we will explore new framework innovations to improve quality, improve latency and create innovative experiences with Android Camera.

1. <font color="#499CD6">打造适应多语言世界需求的产品 (Building for a Multilingual World)</font>  
了解将您的 Android 应用程序国际化的最佳实践以及如何实现每个应用程序的语言首选项。  
Learn best practices to internationalize your android app and how to implement per-app language preferences.

1. <font color="#499CD6">迁移到 Play 结算库 5：在 Google Play 上提供更灵活的订阅服务 (Migrate to Play Billing Library 5)</font>    
Google Play 于 2022 年 5 月添加了新的订阅功能，使您的订阅产品目录更加灵活和复杂。了解如何通过迁移到 Play Billing Library 5 和采用为利用新功能而创建的新端点来调整您的 Android 和服务器集成，并设计您的系统以降低维护成本。  
Google Play added new subscription features in May 2022 that enable more flexibility and complexity in your subscription product catalog. Learn how to adapt your Android and server integrations by migrating to Play Billing Library 5 and adopting the new endpoints created to take advantage of the new capabilities, and design your system to reduce the cost of maintenance.

1. <font color="#499CD6">利用最新的 Android 功能设计高品质的应用 (Designing a High Quality App with the Latest Android Features)</font>    
最新版本的 Android 带来了完全重新设计的 UI，每次点击、滑动和滚动都会让人感觉栩栩如生。在本次会议中，我们将介绍 3 个平台功能，以帮助您使用高级布局、令人愉悦的导航和易于访问的颜色系统来完善您的应用程序。用户期望他们的应用程序获得高质量的体验 - 开发人员如何满足这一要求？  
The recent releases of Android have brought a totally reimagined UI that feels alive with every tap, swipe and scroll. In this session, we will cover 3 platform features to help you polish your app with premium layouts, delightful navigation and an accessible color system. Users expect a high quality experience for their apps - how can developers meet that?

1. <font color="#499CD6">设备端机器学习硬件加速 (Hardware Acceleration for On-Device Machine Learning)</font>    
硬件加速可以显着减少支持机器学习的功能的推理延迟，并允许您提供其他方式可能无法实现的实时设备体验。如今，除了 CPU 之外，Android 设备还嵌入了各种专用芯片，例如 GPU、DSP 或 NPU，您可以使用它们来加速 ML 推理。在本次演讲中，我们将介绍 TensorFlow 和 Android ML 团队提供的一些工具和解决方案，这些工具和解决方案可帮助您利用各种硬件来加速 Android 应用程序中的 ML 推理。  
Hardware acceleration can dramatically reduce inference latency for machine learning enabled features and allow you to deliver live on-device experiences that may not be possible otherwise. Today, in addition to CPU, Android devices embed various specialized chips such as GPU, DSP or NPU that you can use to accelerate your ML inference. In this talk we will go over some tools and solutions offered by TensorFlow and Android ML teams that help you take advantage of various hardware to accelerate ML inference in your Android app.

1. <font color="#499CD6">揭开认证的面纱 (Demystifying Attestation)</font>  
设备信任很复杂，但对于现代应用程序来说必不可少。即使是最大公司中最好的移动开发人员也很少有时间成为专家。在本次演讲中，我们将讨论什么是认证，哪些应用程序应该利用它，如果您不信任设备应该采取什么行动，以及 Play Integrity API 如何简化您提高应用程序安全性的途径。  
Device trust is complicated but essential for modern apps. Even the best mobile developers at the largest companies rarely have the time to become experts. In this talk - we will discuss what attestation is, which apps should be taking advantage of it, what actions you should take if you don’t trust a device, and how Play Integrity API simplifies your path toward improving your app secuirty.

1. <font color="#499CD6">为 Compose 打造无障碍支持 (Building Accessibility Support for Compose)</font>  
Jetpack Compose 是 Android 用于构建本机 UI 的新工具包，在本次演讲中，我们将讨论如何构建一个与无障碍服务兼容的新 UI 工具包。本次演讲旨在帮助开发人员更深入地了解各种无障碍服务（例如 TalkBack 和 Switch Access）如何能够了解和监控 Android 应用程序中的 UI 状态。  
Jetpack Compose is Android’s new toolkit for building native UI, and in this talk, we’ll talk about what it took to build a new UI toolkit to be compatible with accessibility services. This talk aims to help developers gain a deeper understanding of how various accessibility services, such as TalkBack and Switch Access, are able to understand and monitor the state of the UI in an Android app.

1. <font color="#499CD6">在您的语音通信应用中支持 BLE 音频 (Supporting BLE Audio in Your Voice Communication Applications)</font>  
Android 13 引入了对 BLE 音频耳机的支持，并且硬件设备将在明年上市。本次技术会议将重点介绍 Telecom API 如何支持 BLE 音频耳戴设备，以利用高达 32khz 的高质量双向音频、立体声麦克风支持和更多功能。  
Android 13 introduces support for BLE Audio hearables and within the next year hardware devices will be available in the market. This technical session will focus on how the Telecom API can support BLE Audio hearables to make use for high-quality bi-directional audio upto 32khz, stereo microphone support & many more features.

1. <font color="#499CD6">隐私沙盒 (Next Up on the Privacy Sandbox)</font>  
Android 上的隐私沙盒概述，包括我们的 Beta 版及更高版本的计划。了解每个 Privacy Sandbox API 中的新功能以及如何在您的应用或游戏中利用它们。  
Overview of the Privacy Sandbox on Android including our plans for Beta and beyond. Learn about the new features in each Privacy Sandbox API and how to take advantage of them in your app or game.

1. <font color="#499CD6">关于 Android 存储方面的内容 (Everything About Storage on Android)</font>  
持久性是每个移动应用程序的核心元素。Android 提供不同的 API 来访问或公开具有不同权衡的文件。您应该请求 WRITE_EXTERNAL_STORAGE 吗？如何访问共享存储上的图像？在本次会议中，您将能够了解存储的关键概念，并利用最新的 API 来提高开发人员的工作效率和用户的隐私。  
Persistence is a core element of every mobile app. Android provides different APIs to access or expose files with different tradeoffs. Should you request WRITE_EXTERNAL_STORAGE? How can you access an image on shared storage? In this session, you’ll be able to understand the key concepts of storage and take advantage of recent APIs to improve both your developer productivity and users' privacy.

1. <font color="#499CD6">捕捉，回放和分享 10bit 视频 (HDR 10BIT: Capture, Playback, and Sharing 10BIT Video)</font>  
本次演讲将深入 HDR 视频并讨论端到端的过程，包括视频捕捉、编辑、回放和共享。我们可以讨论的具体主题包括用于编辑的新 Media3 Transformer API、在 SurfaceView 上显示图形以供播放，以及准备文件共享的任何其他最佳实践。  
This talk will take a dive into HDR video and discuss the process from end-to-end, including video capture, editing, playback, and sharing. Specific topics we can discuss include the new Media3 Transformer API for editing, displaying graphics on SurfaceView for playback, and any additional best practices to prepare the files for sharing.

1. <font color="#499CD6">通过采用尊重隐私的权限工作流来增强用户信任 (Foster User Trust by Adopting Privacy-respecting Permission Workflows)</font>  
在本次演讲中，我们将重申过去几个版本中 Android 权限方面的一些重大发布，同时解释为什么我们相信上述原则集，并展示我们认为已经在自己的应用程序中采用这些原则的一些我们自己的 Google 应用程序为用户打造更好的隐私体验。我们希望我们可以激励开发人员采用这些最佳实践并增强用户对其应用程序体验的信任。  
In this talk, we’ll reiterate some big launches in Android permissions over the past few releases, while explaining why we believe in the set of principles above and showcase some of our own Google apps that we believe have adopted these in their own apps to build a better privacy experience for their users. We hope that we can inspire developers to adopt these best practices and enhance user trust in their app experiences.

1. <font color="#499CD6">构建桌面小部件 (Building Modern Android App Widgets)</font>  
您的应用程序是否有应用程序小部件或您想构建一个？在本次会议中，我们将展示我们如何使我们的应用程序小部件现代化以提高参与度，我们将分享重要技巧以帮助您构建现代 Android AppWidgets  
Does your app have an app widget or you want to build one? In this session we will showcase how we modernized our app widgets leading to more engagement and we will share top tips to help you build Modern Android AppWidgets

1. <font color="#499CD6">防止您的应用程序在仅 64 位的世界中失败 (Keep your App From Failing in a 64-bit Only World)</font>  
64 位只是在人们使用该平台的方式上打开了一些空白。本次演讲涵盖如何确保您的应用程序适用于下一代 Android 设备。  
64-bit only opens up some gaps in the way people have been using the platform. This talk covers how to make sure your app will work on the next generation of Android devices.

1. <font color="#499CD6">Android 上的超宽带介绍 (Introduction to Ultrawide-band on Android)</font>  
介绍超宽带技术、关键概念和现实生活中的应用。还介绍了我们新的 Jetpack 库，以使用它和示例代码构建适用于 Android 的应用程序。  
Introduction to Ultrawide-band technology, key concepts and real life applications. Also a walkthrough of our new Jetpack library to build apps for Android with it and sample code.

1. <font color="#499CD6">同步 Health Connect 数据 (Syncing Data with Health Connect)</font>  
健身应用程序将活动存储在数据库中。Health Connect 还将活动存储在数据库中（不同的数据库！）。如何以一致、可靠且用户可理解的方式在两者之间同步活动（包括传播删除和更新）？  
Fitness apps store activities in a database. Health Connect also stores activities in a database (a different one!). How can activities be synced between the two (including propagating deletes and updates) in a way that is consistent, reliable, and comprehensible to the user?

1. <font color="#499CD6">Android 显卡 (Android Graphics)</font>  
快来学习如何在您的应用程序中使用 Android 的图形 API，包括利用一些最新的平台功能，例如 AGSL、Android 图形着色语言。  
Come learn how to use Android's graphics APIs in your app, including taking advantage of some of the newest platform features such as AGSL, Android Graphics Shading Language.
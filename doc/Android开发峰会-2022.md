#  Android 开发峰会 笔记

# 主要分为三个部分： Modern Android Development , Form Factors , and Platform

## 一、Modern Android Development 现代安卓开发

### 1. Compose 中的自定义布局和图形
例子是尝试编写一个Sleep Tracker(睡眠状态展示)

学习了如何在Compose中进行自定义操作，使用自定义布局及其自身的测量结果和放置逻辑，以非常动态的方式绘制条形。同时还了解了不同API，可以使用这些API来绘制自定义对象并为其设置动画

[源码](https://goo.gle/compose-jetlagged)

[了解有关布局的更多信息](https://goo.gle/compose-custom-layouts)

[了解有关图形的更多信息](https://goo.gle/compose-graphics-docs)

### 2. Compose 修饰符深入探讨 -- Modifier
介绍Modifier的更新迭代过程以及在迭代过程中的出现的问题及其解决方案，讨论最终形态Modifier.NODE

### 3. UI 层中的状态持有者和生产状态



### 4. 使用Baseline Profiles(基线配置文件)使应用程序快速运行



### 15. Jetpack Compose 的更多性能提示
1. 确保应用在发布模式下运行，并且要启动R8优化功能
* 调试版本运行速度慢，因为安卓会关闭优化功能来进行调试体验：
  * 为了逐行检测代码，缩减功能不会启用

2. 每次重组都会刷新问题，使用remember来缓存开销大的操作或分配，来确保这些操作只有在需要的时候运行。或者将这些数据转移到 viewmodel或者datasource

3. LazyColumn在滑动过程中，会出现重组，需要增加一个键值（**key**）就可以使得重组只针对当前滑动的范围，为不是所有列表项

4. 衍生修改（Deriving change)
```Kotlin
// 下面实现功能是检测列表滑动状态，当列表滑动后，显示一个按钮，用于返回顶部功能
// listState就是为了获取当前列表的状态，而在滑动过程中会一直触发，从而导致这个组合一直被刷新
// 引入**derivedStateOf**就是为了将繁忙的信息流转换成Boolean条件。

val listState = rememberLazyListState()
LazyColumn(state = listState) { ... }
val showButton = remember {
    derivedStateOf {
        listState.firstVisibleItemIndex > 0
    }
}
AnimatedVisibility(visible = showButton) { ... }
```
5. 拖延（Procrastination)
```Kotlin
// 下面是一个背景在青色和洋红之间循环变化的实现
// color的变化会引起该组合函数的不断刷新
val color by animateColorBetween(Color.Cyan, Color.Magenta)
Box(Modifier.fillMaxSize().background(color))
```
Compose 包含三个部分:组合（Composition），布局（Layout）， 绘制（Draw）
* Composition: calls composable funtion to determine what the user interface shold be
(系统会执行可组合函数)，在此期间，系统会创建或者更新应用的内容，
* Layout: Measures the user interface elements and determines where to place them on the screen(系统会测量由组合定义的内容并确定要将这些元素放在屏幕上的哪些位置)，这个阶段要考虑所有修饰符及对其他可组合函数的所有调用，这些函数包括Text/Row/Column等，并会在单词传递中测量和放置所有内容
* Draw: Draws the user interface elements to the screen(系统会发出实际图形指令，将内容会知道应用的画布)，这些指令涉及内容包括绘制线条、弧形、举行、图片和文字，并由上一阶段layout确定的位置绘制这些图元

这三个阶段会在他们读取的数据发生变化的每一帧重复执行，但是如果数据没有变化，就可以跳过这三个阶段中的一个或多个

Reading state：将状态的读取一直延迟到需要的时候
```Kotlin
// 引入drawBehind，drawBehind接受在Compose的绘制阶段调用的函数实例。
// 由于这事唯一一次读取颜色值，在颜色发生变化时只有绘制阶段的都结果需要更改。
// 这样，绘制阶段就编程了唯一需要重新执行的阶段，让Compose可以跳过组合和布局这两个阶段
// 这里的绝妙指出是在函数实例中读取颜色状态，而不是在组合函数中读取。由于函数实例没有变化，因此它读取的变量保持不变，从组合的角度来讲没有任何变化，因此不需要重新执行此函数
val color by animateColorBetween(Color.Cyan, Color.Magenta)
Box(
    Modifier
      .fillMaxSize()
      .draBehind {
        drawRect(color)
      }
)
```

6. Running backwards： 向后运行
```Kotlin
// 设计人员要求我们显示银行交易列表和余额

var balance by remember { mutableStateOf(0) }
balance = 0
for (transaction in transactions) {
    Row {
        balance += transaction
        Text("Transaction: $transaction Balance: $balance" )
    }
}
```
上面代码存在问题出在更新余额的代码行，这违反了Compose的核心假设（Compose假设：值一旦被读取在完成之前会保持不变），你绝对不应对已存在组合中读取的值进行写入，对已读入的值继续写入，正是我们所说的向后写入(Backwards write)，会导致每一帧都发生重组

7. Baseline Profiles: 基准配置文件
有助于加快启动速度、减少卡顿、提高性能 Speed up startup and hot paths

[生成自己的基准配置文件以及如何配置Macrobenchmark来生成和测试配置文件](https://goo.gle/baseline-profiles)

[源码](https://goo.gle/baseline-profiles-codelab)

[performance](https://goo.gle/compose-performance)

Tips：小结
* 介绍了如何配置应用以获得最佳性能 -- 需要正式环境和R8优化
* 介绍了一些常见错误几修复方案
  * remember{} -- Caches calculations and allocations
  * LazyList Key -- Helps LazyList know what has changed in a list
  * derivedStateOf{} -- Avoids unnecessary recomposition for state changes we need
  * Defer reads(延迟读取) -- Read state only when required
  * Backwards writes  -- Never write to state that has already been read
  * Baseline Profiles -- Optimises startup and other critical paths
* 

------------------------------------------------------
***性能优化的关键就是检查、改进、监控和重复这三个步骤***  
***性能优化的关键就是Inspect、Improve、Monitor***

[如何编写基准](https://goo.gle/madskills-inspecting-performance)  
可以观看YouTube上的MAD Skills Inspecting Performance视频

另一个可以用来帮助查找应用程序性能问题的工具是跟踪(https://goo.gle/compose-tracing)  

8. Defer reading state: 延迟于都状态
可以借用Android Studio自带工具中的Layout Inspector进行分析，可以查看当前应用的recomposition count和skiped count(这个会导致应用卡顿)
Compose分为三个阶段Composition，Layout和Draw
Composition是通过构建可组合项树来确定要显示的内容；
Layout是采用该树并计算出它们将在屏幕上的显示的位置位置；
Draw顾名思义就是将它全部绘制到屏幕上

移除Composition阶段，可以提高效率，可以采用延迟读取的方法。
而如果直接使用参数传值的方法，不是最好的处理方式，
正确处理读取延迟方法
```Kotlin
// 1. 原始code. offset变化会导致整个Parent改变
@Composable
fun Parent() {
    val offset by animateFloatAsState(targetValue = 10f)
    Column {
        Header()
        Child(offset)
        Footer()
    }
}
@Composable
fun Child(offset: Float) {
    Box(Modifier.offset(y = offset))
}

// 2.改为传参的方式,将实际状态对象传递给子可组合对象
// 虽然这样子确实会延迟读取状态，但它反过来会使得Compose代码更难使用。例如：无法在使用byDelegate语法，并且必须每次都读取添加.value方法，还将可组合项绑定到需要使用状态，并且无法在传递固定值
@Composable
fun Parent() {
    val offset = animateFloatAsState(targetValue = 10f)
    Column {
        Header()
        Child(offset)
        Footer()
    }
}
@Composable
fun Child(offset: State<Float>) {
    Box(Modifier.offset(y = offset.value))
}

// 3.正确处理延迟获取的方法，是使用lambda
// 通过lambda可以控制何时读取状态，而不会过多影响其余代码，同时仍然可以在lambda中使用by property Delegate来取值
@Composable
fun Parent() {
    val offset by animateFloatAsState(targetValue = 10f)
    Column {
        Header()
        Child({ offset })
        Footer()
    }
}
// Deferring read with lambda
@Composable
fun Child(offset: () -> Float) {
    Box(Modifier.offset(y = offset().dp))
}


// 4. 上面第三种方法已经可以做到了延迟读取，但是我们可以做的更好
// if we can read this lambda inside a modifier that isn't run during composition, we can skip composition altogether. This is because we won't be changing our composition tree at all.
// 如果我们可以在Composition期间不运行的Modifier中读取这个lambda，我们可以完全跳过Composition。这是因为我们根本不会改变我们的Composition tree。
@Composable
fun Parent() {
    val offset by animateFloatAsState(targetValue = 10f)
    Column {
        Header()
        Child({ offset })
        Footer()
    }
}
// Deferring read with lambda into Modifier
@Composable
fun Child(offset: () -> Float) {
    Box(Modifier.offset {
        IntOffset(x = 0, y = offset().toInt())
    })
}
```
[修复Jetsnack中的问题现场演示和调试重组](https://goo.gle/compose-debug-recomposition)  

9. Stability 稳定性
```Kotlin
// 1. 原始
@Composable
fun HomeScreen(...) {
    val contact by vidwModel.contact.collectAsState()
    var selected by remember { mutableStateOf( false )}
    Column {
        Checkbox(selected, { selected = it })
        ContactDetails(contact)
    }
}
```
[稳定性方面相关信息](https://goo.gle/compose-stability-explained)  

10. derivedStateOf{}  ≈ distinctUntilChanged
使用场景：在UI使用时，状态或键变化超过我们所需要的更新。
State --> derivedStateOf{} 和 remember(key){}
derivedStateOf{} : State is changeing more than you want to update your UI(状态变化超过你想要的UI更新)
remember(key){} : State needs to change as much as keyi changes

11. reportFullyDrawn 是关于一个Activity的API
Report to the system that your app is now fully drawn, for diagnostic and optimization purposes.(它向Android系统发出您的应用程序已经准备好使用的信号，可以实现android预加载I/O调用来优化您的应用程序启动)

```Kotlin
implementation "androidx.activity:activity-compose:1.7.0-alpha01"

@Composable
fun ReportDrawnWhenSample() {
    var contentComposed by remember { mutableStateOf(false)}
    LayzyColumn {
        item {
            contentComposed = true
            Text("Hello World)
        }
    }

    // 新增reportDrawnWhen可组合函数，等待执行到contentComposed为true后才执行
    ReportDrawnWhen { contentComposed }
}

@Composable
fun ReportDrawnAfterSample() {
    val lazyListState = rememberLazyListState()

    ReportDrawnAfter {
        lazyListState.animateScrollToItem(10)
    }

    lazyColumn(state = lazyListState) { ... }
}
```

Tips: 小结
* Macrobenchmark : THe answer to if your performance optimisations are working.
* Defer reading state : Use lambdas to read state as late as possible
* Stability : Compose determines the stability of your Composeables to determine if they can be skipped.
* derivedStateOf{} : Used when your state of key is changeing more than you need to update your UI.
* reportFullyDrawn : Allows Android to optimise your apps startup

### 18. 5 个快速动画让您的 Compose 应用脱颖而出
动画效果的体验  
[动画](https://goo.gle/compose-animation)  
[动画欺骗](https://goo.gle/compose-animation-cheatsheet)  
[Blog](https://goo.gle/animated-content-blog)  

### 24. 编写您的第一个 Compose UI 测试
```
androidTestImplementation "androidx.compose.ui:ui-test-junit4:@compose_version"
debugImplementation "androidx.compose.ui:ui-test-manifest:@compose_version"
```

| Semantics Node | Matchers |
| -- | -- |
| **Properties** |  |
| Text |  hasText |
| ContentDescription | hasContentDescription |
| ... |  |
| **Actions** |  |
| onClick |  hasClickAction |
| setText | hasSetTextAction |
| ... |  |
| **Parent** |  hasParent |
| **Children** | hasAnyChild |


## 二、Form Factors 外形尺寸

### 13. 插图：Insets: Compose Edition
针对 Status Bar 和 Navigator Bar 等进行合理的布局，以便适应手机和平板等不同屏幕的设备
1. 操作步骤
```kotlin
// 1. 在 Activity 的 onCreate 函数中添加下面代码  
WindowCompat.setDecorFitsSystemWindows(window, false)
// 2. 在 AndroidManifest.xml 中添加  
android:windowSoftInputMode="adjustResize"
// 3. 在 themes.xml 中将状态栏和导航栏的背景颜色改为透明  
<item name="android:statusBarColor">@android:color/transparent</item>  
<item name="android:navigationBarColor">@android:color/transparent</item>
// 4. 使用的时候，修饰符中增加一个填充修改器 windowInsetsPadding , 然后使用的dp是WindowInsets.statusBars, 
Box(Modifier.windowInsetsPadding(WindowInsets.statusBars)) {}
```
2. 上面可用类型包括:::  
Platform Insets |  "Safe" Insets
= | =
WindowInsets.statusBars | WindowInsets.safeDrawing
WindowInsets.navigationBars | WindowInsets.safeGestures
WindowInsets.ime | WindowInsets.safeContent
WindowInsets.waterfall |
WindowInsets.captionBar |
WindowInsets.tappableElement |
...and more ! |

3. windowInsetsPadding已经被外部使用了，那么内部的windownInsetsPadding不会在起作用
4. Material3 组件提供了一些开箱即用的 Insets 支持
```kotlin
import androidx.compose.material3.*
// 脚手架
Scaffold(contentWindowInsets = ScaffoldDefaults.contentWindowInsets) {/* ... */}
// 导航栏
NavigationBar(contentWindowInsets = NavigationBarDefaults.contentWindowInsets) {/* ... */}
// 导航卷轴
NavigationRail(contentWindowInsets = NavigationRailDefaults.contentWindowInsets) {/* ... */}
ModalDrawerSheet(contentWindowInsets = DrawerDefaults.contentWindowInsets) {/* ... */}
// 顶部状态栏
TopAppBar(contentWindowInsets = TopAppBarDefaults.contentWindowInsets) {/* ... */}
```
5. 代码示例 [https://github.com/android/nowinandroid](https://github.com/android/nowinandroid)
6. Additional APIs 
``` 
Modifier.consumedWindowInsets
Modifier.withConsumedWindowInsets
Modifier.windowInsetsStartWidth
Modifier.windowInsetsEndWidth
Modifier.windowInsetsTopHeight
Modifier.windowInsetsBottomHeight
windowInsets.asPaddingValues()
windowInsets.exclude
windowInsets.add()
```
7. More Resources
```
https://goo.gle/compose-insets-docs
https://google.github.io/accompanist/insets/#migration
```
### 19.为您的 Android 应用程序添加手写笔支持----Adding Stylus support to your Android app
1. Low-Latency Jetpack Library 
* An easy way to implement stylus-friendly surfaces 该库可以轻松实现手写笔友好的用户体验
* Leverages front and multi buffered rendering 利用前端和多缓冲渲染的组合来获得低延迟图形和高质量渲染避免产生视觉撕裂伪影
* Supports Android Q+
* Reduces graphics latency by 50% or more 将图形延迟减少50%甚至更多

2. 如何工作
3. 实现: 使用我们定义的类型 MyData 用于渲染到前缓冲层和多缓冲层的输入数据，从而创建一个 GL 前缓冲渲染实例。然后我们提供一个表面实例 mySurfaceView 作为多缓冲渲染层以及前端缓冲层的父层。最后我们提供了  GLFrontBufferedRenderer 回调的方式指定我们的 GL 渲染逻辑。
```
GLFrontBufferedRenderer<MyData> ( // MyData -> Tmeplate type
    mySurfaceView,            // Parent Layer
    myCallbacks               // Render callbacks
)

// 回调实现
val callbacks = object: GLFrontBufferedRenderer.Callback<MyData> {
    override fun onDrawFrontBufferedLayer(
        eglManager: EGLManager,
        param: MyData
    ) {
        // OpenGL code to render delta of stroke
    }

    override fun onDrawDoubleBufferedLayer(
        eglManager: EGLManager,
        params: Collection<MyData>
    ) {
        // OpenGL code to render entire stroke
    }
}
```
## 三、Platform 平台
### 1. 将应用迁移到 Android 13

### 2. 为所有用户呈现高质量的媒体体验----Presenting a high-quality media experience for all users
1. JetPack Media3
![](./2022%20Android%20Dev%20Summit/screencap/3.Platform.02.WhyMedia3.png)

> ExoPlayer: Default Player implementation in Media3
```Kotlin
val player = ExoPlayer.Builder(context).build().apply {
    addListener(MyListener())
}
val mediaSession = MediaSession.Builder(context, player)
    .setId(MY_MEDIA_SESSION_TAG)
    .setSessionCallback(MyCallback())
    .build()
```
2. 可以观看之前的视频：
[https://goo.gle/ads21-media](https://goo.gle/ads21-media)
[https://goo.gle/io22-media](https://goo.gle/io22-media)

3.自定义MediaPlayer
```Kotlin
Implementing a custom Player with Media3

class CustomPlayer : SimpleBasePlayer(looper) {
    override fun getState(): State {
        // Set available Commands
        // Configure playWhenReady, mediaItemIndex, currentPosition, etc.
    }
    // Implement methods required by available Commands
}
```
4. Performance Class

### 04.建设多语言世界----Building for a multilingual world
每个国家有不同的语言，同一个国家也可能存在多种语言情况，而用户选择上肯定选择自己理解的或者喜欢的语言
* Organize app resources 组织应用程序资源
 -- so users receive information they understand  以便用户收到他们理解的信息
资源文件命名上使用 Language Script Region 三个来区分

 values/strings.xml
 values-zh/strings.xml 中文
 values-zh-rTW/strings.xml 繁体
 values-zh+hant/strings.xml 繁体
 zh-TW 如果没有会回退到 zh-Hant 不会回退到 zh
* Define resourceConfigurations 定义资源配置
  -- so layouts are consistent 以便和布局保持一致
假如用户将设备语言中首选项这只为日语，第二个是英语，那么如果apk中不支持日语时，会自动显示英语

Android studio功能：Build -> Analyze APK中选择具体apk，然后选择"resources.arsc",再选择"string"后，右边就可以看到这个apk有多少中语言配置
```
// Solution: define resourceConfigurations
// build.gradle 添加下面是为了传递我们的应用程序支持的语言列表，这样子就会使得apk的大小减少
android {
    defaultConfig {
        ...
        resourceConfigurations {
            ["en", "zh", "zh-rTW", "b+zh+hant"]
        }
    }
}
```
* Avoid concatenating and reusing short strings 避免连接和重复使用短字符串
  -- so users have a great experience across languages 这样用户就有了很好的跨语言体验
    * Problem: concatenating ,比如使用中间带空格的，英语形容词在名词前面，但是西班牙语名词是在形容词前面，而对日语来说没有空格的说法
    * Problem: reusing short strings， 比如off，原先表示开关的，但是葡萄牙语会翻译成feminine，但是当前开关状态是masculine，这样子使用off查找是找不到的。

* Use MessageFormat 使用消息格式
  -- for flexible string formatting across languages 用于跨语言的灵活字符串格式
英语的倒计时是使用 One 或者 Other， One 表示一天，“Last day!", Other 表示 2天，3天... "%d days left", 而在俄语中 One 可能表示1，21， 31， 41等，这样子就会出现问题

![](./2022%20Android%20Dev%20Summit/screencap/2022-12-08_160900.png)
![](./2022%20Android%20Dev%20Summit/screencap/2022-12-08_162530.png)

Other ways to use MessageFormat for flexible string formatting across languages 
cardinal numbers,ordinal numbers,dates,times, gender, nesting messages
考虑到用于序号、日期、时间、性别 和嵌套复杂的消息格式

* Enable per-app language perferences 启用每个应用程序的语言偏好

> Learn More
> [goo.gle/app-languages](https://goo.gle/app-languages)
> [goo.gle/app-languages-video](https://goo.gle/app-languages-video)


### 12.关于 Android 存储的一切----Everything about storage on Android













## 一、Modern Android Development : *现代 Android 开发*

### Modern Android Development
1. 在build.gradle中添加 compose-bom 依赖包，可以直接将依赖的所有compose版本信息移除，bom会自动提供最新的稳定版jar
2. 在layout inspector 中可以查看compose的重组次数等信息
3. 为了提高android运行的性能问题，可以借助工具库
- Benchmark ： 通过引入Macrobenchmark,监控应用的启动和运行时的性能。可以检测电池使用，并向compose代码添加详细的构成计时
- Fragment 1.4 引入了新的严格模式以自动捕获关于fragment的常见问题
- Activity 1.6 增加了对Android 13的预测性返回的支持

> 提升性能方面，具体观看[利用基准配置文件加速应用的运行]

4. 提升性能，推出了App Quality Insights功能，使得开发者能过通过字段查看崩溃报告，还可以导航到有问题的代码
- 使用Play Track筛选问题，并快速识别需要处理的新问题

> ZEPETO 是一个3D头像服务，在使用了Compose后简化了至少1600行代码，推荐使用compose


### Wear OS 3 ： 穿戴式设备，如手表

构建一个游泳跟踪应用，记录游泳圈数。

> Strava 专门为运动员打造的平台
developer.android.com/wear

### Lager Screen 大屏设备，如折叠设备，平板电脑和Chrome OS设备


### Android 平台新功能
增强了 Personalization 个性化、Privacy 隐私性，Security 安全性，Connectivity 连接性，Mdia 媒体功能

1. Personalization: 例如语言，增加了语言的偏好设置，

[学习compose的路线图](https://developer.android.com/jetpack/androidx/compose-roadmap)





## 二、Form Factors : *结构因素*  2022年11月9日

## 三、Platform : *平台*  2022年11月14日



# $\color{#499CD6}{2022 Android 开发者峰会}$
* 现代 Android 开发 (Modern Android Development)  
![Modern Android Development](./2022%20Android%20Dev%20Summit/dcim/01.modern%20android%20development.png "Modern Android Development")
* 设备类型 (Form Factors)  
![Form Factors](./2022%20Android%20Dev%20Summit/dcim/02.Form%20Factors.png "Form Factors")
* 平台 (Platform)  
![Platform](./2022%20Android%20Dev%20Summit/dcim/03.Platform.png "Platform")

## $\color{#499CD6}{现代 Android 开发 (Modern Android Development)}$
Watch the deep technical sessions on your favorite Modern Android Development tools and APIs.  
观看您最喜爱的现代Android开发工具和API的深度技术会议。  
![现代Android开发](./2022%20Android%20Dev%20Summit/dcim/04.top3picks_01.png "现代Android开发")
### $\color{#499CD6}{Custom Layouts and Graphics in Compose}$
Jetpack Compose offers a variety of out-of-the-box solutions to quickly and easily build screens from scratch. But what happens when you need to go a step beyond and go fully custom? In this talk, you’ll learn how to create complex designs using a powerful combination of custom Compose Layouts and Graphics. We will cover things like laying out a custom graph, Compose drawing operations, and animations through a more hands on approach by building an intricate Sleep Tracker sample app in just 20 minutes.

### $\color{#499CD6}{Compose Modifiers Deep Dive}$
A deep dive into the history of Modifiers and the constraints of APIs. As well as the problems they are meant to solve leading up to a major overhaul in implementation in 1.3 and the addition of several lower level, but powerful, experimental APIs that we will be migrating to over the course of the next few releases. This talk will go into the why’s and how’s of this migration, how it impacts developers, and the performance impact it will have for end users.

### $\color{#499CD6}{State Holders and State Production in the UI Layer}$
The UI layer displays application data on the screen. But how is it done exactly? In this talk, we'll dive deep into the UI state production pipeline and state holders that manage UI complexity. As well as, get to know the differences between UI and business logic, a ViewModel and a plain state holder class, state and events, and more! What is all that, when to use which, and how to do it.

### $\color{#499CD6}{Making Apps Blazing Fast with Baseline Profiles}$
Baseline Profiles are a new way to dramatically improve app startup and runtime performance of apps and libraries. In this session we will share how to create a Baseline Profile and verify its effectiveness. You will also discover how the Android Runtime improves app performance, when a Baseline Profile is provided on various Android platform versions.

### $\color{#499CD6}{State of the Art of Compose Tooling}$
In this talk, we will walk you through the state of art of Compose Tooling in Android Studio by showing how to incorporate these tools into your development workflow.

You’ll learn to design and validate your UI with Compose Preview, accelerate your development workflow with Live Edit, and write code faster with Compose editing features. We’ll also show you how to analyze your layout, understand recompositions with the Layout Inspector, and pinpoint performance issues in your code.

After this talk, you will be ready to leverage these tools to build beautiful, performant, and adaptive Compose UI.

### $\color{#499CD6}{What's New in Android Build}$
In this talk, we would like to share what is new in Android Gradle Plugin (AGP), and how the new APIs and features can help you with Build productivity (maintenance and speed).

### $\color{#499CD6}{From Views to Compose: Where Can I Start?}$
Using Jetpack Compose doesn’t mean you need to rebuild your app from scratch, instead, you can take an incremental approach to migrating. In this talk, you will learn how to start introducing Compose into your codebase and how to gradually migrate existing screens. After this talk, you’ll have a solid foundation on how to approach converting your app to Compose.

### $\color{#499CD6}{Where to Hoist that State in Compose?}$
In this talk, you'll learn how and where to hoist state in Jetpack Compose. When should state be hoisted? Should that be in a composable function, plain state holder class, or in a ViewModel? In this session, we'll explore the different possibilities using real-world examples.

### $\color{#499CD6}{Material You in Compose Apps}$
The Material 3 Jetpack Compose library will be stable at ADS! Learn about new and updated theming and components, and get started using the library in your production apps. This talk also covers Material You dynamic color and migrating from Material 2. Come see why adopting Jetpack Compose now makes apps feel fresh and helps stay in sync with the evolution of the Android OS visual language and aesthetic.

### $\color{#499CD6}{5 Ways Compose Improves UI Testing}$
If you need another excuse to migrate your app to Compose, testing composables is easier, faster and more reliable than testing Views. In this talk, we'll look at five ways testing has improved thanks to how Compose was designed.

### $\color{#499CD6}{Type Safe, Multi-Module Best Practices with Navigation Compose}$
As your app grows in size and complexity, following these best practices for using Navigation Compose will set you up for expanding your navigation graph across multiple modules in a way that maintains type safety across all navigation calls. This talk will also explain how to separate Kotlin Multiplatform ready screens from Navigation code and how to bring your Navigation code back together after splitting it across multiple modules.

### $\color{#499CD6}{Practical Room Migrations}$
Database migrations may seem like an extreme sport at times - if you agree, this is the talk for you! In this talk, we will cover auto migrations, how to migrate a pre-populated database, how to pre and post process data for migrations, and how to handle foreign keys & views during a migration. With these new skills, migrations won’t feel like skydiving with a parachute anymore - but will perhaps, feel like skydiving with a Jetpack!

### $\color{#499CD6}{Test at Scale with Gradle Managed Devices}$
Gradle Managed Devices (GMD) makes it easy to leverage virtual devices for scalable, fully-managed testing, with test caching, sharding, and lifecycle management built in. We’re now adding support for both physical and virtual devices running in Firebase Test Lab to bring the benefits of GMDs to Google’s cloud testing solution for Android.

### $\color{#499CD6}{5 Android Studio Features You Don't Want to Miss}$
By now everyone has probably seen Jetpack Compose tools, Live Edit, and other high profile features of Android Studio in action. That's why in this talk we'll show you 5 upcoming features and improvements in the IDE that are perhaps not as easily noticed, but have a chance to greatly improve your everyday development workflows.

### $\color{#499CD6}{More Performance Tips for Jetpack Compose}$
A follow up to the Common Performance Gotchas I/O in Jetpack Compose talk. We will go further into the details of why deferring reads of Compose state works, learn about stability and how Compose infers it, have a look at a new API for reportFullyDrawn, and more.

### $\color{#499CD6}{Building a Scalable, Modularized, Testable App From Scratch}$
If you're building an app from scratch or looking to update your app to follow modern Android development best practices, this talk will give you a high-level overview of all the pieces you need, and how they fit together using a real-world example: the Now in Android app.

This talk will also explain how we built one of the app's features and the decisions behind its design. We'll cover the app's testable, modular architecture and talk about how we built a set of reusable UI elements using Jetpack Compose and Material3.

### $\color{#499CD6}{Reimagining Designer-Developer Handoff: Introducing Relay}$
In this lightning talk we’ll introduce you to Relay, available now in open alpha. Relay is a new process that allows teams to create UI in Figma and generate high-fidelity Compose UI components. Relay puts structured component data at the center of the collaboration between designer and developer, resulting in instant UI implementation and rapid iteration.

### $\color{#499CD6}{5 Quick Animations to Make Your Compose App Stand Out}$
Want to add some movement to your Jetpack Compose app, but don’t have the time to learn everything there is to know about Animations? Here are 5 quick animations to make your app come to life in just a few minutes!

### $\color{#499CD6}{Styling Text in Compose}$
Text styling can give your app character. In this talk, we’ll use Jetchat to learn how to use Material APIs to configure typography, including using downloadable fonts and variable fonts. We’ll then customize our chat bubble so it collapses depending on how long the message is. We’ll end by styling the message box: giving it a gradient border, a cursor that changes color as you type, and a fully custom decorated box.

### $\color{#499CD6}{Create Offline-First Apps}$
No network? No problem! Learn how to build an offline-first app. This talk will cover modeling, data access semantics, synchronization, and conflict resolution. It will also highlight the utilities and data structures that are indispensable when building offline first apps.

### $\color{#499CD6}{By Layer or Feature? Why Not Both?! Guide to Android App Modularization}$
This practical talk will give you a set of common patterns and recipes for modularizing your project in the context of the modern Android app architecture. Learn the types of modules and their role in a multi-module codebase.

### $\color{#499CD6}{Collecting Flows in a Lifecycle-Aware Manner}$
Collecting flows in a lifecycle-aware manner is the recommended way to collect flows on Android. In this talk, we'll explore the different APIs you have to do so, such as the repeatOnLifecycle API or collectAsStateWithLifecycle API in Jetpack Compose, and see how they work under the hood.

### $\color{#499CD6}{Accurately Measure App Performance with Profileable Builds}$
During local development, most app developers build and run their app in debuggable mode. However, debuggable apps incur significant and varied performance degradation and are not useful for measuring timing accurately. In this talk, learn the benefits of profileable apps for performance measurement and how to build them in Android Studio.

### $\color{#499CD6}{Write Your First Compose UI Test}$
In this talk we’ll walk you through writing your very first Compose UI test. We’ll cover finders, assertions, actions, and matchers and take a quick look at the semantics tree.

### $\color{#499CD6}{Address Firebase Crashlytics Reports Faster from Android Studio}$
Firebase Crashlytics records the errors that happen in developers' production apps, but up until now you needed to go to Crashlytics’s web console to investigate the errors. App Quality Insights, introduced in Android Studio Electric Eel, makes it available to integrate the errors with Android Studio, allowing you to navigate to the relevant code that causes the errors.

This talk will explain the basics of App Quality Insights and how that can be useful in debugging errors in production apps.

## $\color{#499CD6}{设备类型 (Form Factors)}$
Watch the videos to learn about the latest updates on building for different form factors and screens.  
观看视频，了解不同形状和屏幕的建筑最新更新。  
![设备类型](./2022%20Android%20Dev%20Summit/dcim/04.top3picks_02.png "设备类型")

### $\color{#499CD6}{Build Better UIs Across Form Factors with Android Studio}$
Android Studio is making it easier and faster to expand your app across multiple form factors--from small to large! Come take a tour across the IDE where we will walk you through new tools and improved features such as Visual Linting, Reference Devices, Resizeable and Wear Emulators, Wear Pairing Assistant, Form Factor Previews, and many more! After this talk, you will be ready to accelerate your workflow with Studio’s multi-device environment to build for Large Screens and Wear OS.

### $\color{#499CD6}{Compose: Implementing Responsive UI for Large Screens}$
Learn how to build adaptive layouts for every screen size. You’ll develop the mindsets for building UI with Compose to create a great user experience across phones, tablets, foldables and ChromeOS devices.

### $\color{#499CD6}{Do’s and Don’ts: Mindset for Optimizing Apps for Large Screens}$
Come learn best practices for building your Android application so it will work well on larger screens and foldables! We'll cover everything from new Android Studio tools, new and updated Jetpack libraries, and more specific design and development guidance to make it easy for you to capitalize on the more than 270M active large screen Android devices!

### $\color{#499CD6}{Designing for Large Screens: Canonical Layouts and Visual Hierarchy}$
Canonical layouts provide a great starting point for differentiated large screen experiences, covering common use-cases and screen sizes. But how do you choose the right layout for your app, or build on top of canonical layouts to create an adaptive experience that’s perfectly matched to your product? Learn to understand canonical layouts from a design perspective and the core development concepts, unpacking the rationale of feeds, list-detail, and supporting panel layouts and unlocking the potential to level up your adaptive design.

### $\color{#499CD6}{Building Media Apps on Wear OS}$
In this talk you will learn how to build a high quality Media app on Wear OS. We will first go through the Core User Journeys for media apps to outline what to build; we will then discuss how to ease the development by adopting our newly released Media Toolkit and Media3 Exoplayer, and we will conclude with some tips and tricks to ensure good performances.

### $\color{#499CD6}{Deep Dive into Wear OS App Architecture}$
Building for Wear OS doesn’t mean learning Android from scratch. This talk will teach you how to add a new Wear project to an existing mobile project, or how to create and structure a Wear app from scratch. We’ll see how best to organize your code to reuse as much as possible, and understand how to leverage tools like Horologist to provide a solid experience for your users.

### $\color{#499CD6}{Creating Helpful Fitness Experiences with Health Services and Health Connect}$
Modern health and fitness experiences exist across multiple form factors. Rarely does data live and die on a single wearable, phone app, or piece of equipment. And it just so happens that a large portfolio of devices, including smartphones and wearables, and many health, fitness, and wellness apps run on Android.

In this talk, you’ll learn how to build cohesive, thoughtful experiences that bridge Health Services and Health Connect and empower users to have control over their data and privacy.

### $\color{#499CD6}{Improving the TV User Experience}$
The latest platform updates for TV offer some great new ways to deliver better user experiences to apps in the living room.

### $\color{#499CD6}{What’s New with the Car App Library}$
Learn about the new features that have recently been added to the Car App Library to make driving optimized apps better than ever on both Android Auto and Android Automotive OS!

### Do More with Multi-Window and Activity Embedding}$
We used to think that users will see and interact with one activity at any given moment. Starting with Android 12L that assumption is not valid any more as Android 12L+ brings multi-tasking front and center allowing the users to have two activities on the screen either from different apps or the same app. This talk will cover what you need to do in order to make sure your app can be launched in multi-window and how to take advantage of the extra real estate and show more than one activity at the same time.

### $\color{#499CD6}{Your Camera App on Different Form Factors}$
Historically, your app could have lived in the same window and with a fixed orientation for its whole life cycle. But with the availability of new form factors, such as foldable devices, and new display modes such as multi-window and multi-display, you can't assume this will be true anymore. Let's see some of the most important considerations when developing an app targeting large screen and foldable devices.

### $\color{#499CD6}{Navigation Compose on Every Screen Size}$
Writing a single navigation system that can handle phones, ChromeOS devices, and everything in between can appear daunting. We’ll talk through strategies for approaching this work and how Navigation Compose can be used alongside the canonical layouts to build the best experience for large screens that seamlessly adapts down to phone screens.

### $\color{#499CD6}{Insets: Compose Edition}$
Don’t be afraid to go edge-to-edge! Learn how insets communicate to your app where system decorations are placed, and how the new Compose APIs help your content automatically move with the system bars, software keyboard, and the taskbar.

### $\color{#499CD6}{The Key to Keyboard and Mouse Support across Tablets and ChromeOS}$
Android has over 270 million active large screen devices today. With every new large screen device introduced, the importance of optimizing your app for keyboard and mouse support continues to grow. This talk dives into the code you can use to introduce and optimize keyboard and mouse support in your app.

### $\color{#499CD6}{Developing for Assistant across Devices}$
In this talk, you'll learn how to take advantage of voice-first APIs and tooling in Android Studio to bring voice functionality through Google Assistant to your apps for various device types.

### $\color{#499CD6}{Three Tiers of Large Screen Quality on Google Play}$
The rising popularity of tablets and foldable devices unlocks opportunities to address a new range of users in innovative ways. A responsive UI allows you to easily build this experience.

In this talk, you’ll get an understanding of what’s available for developers to support large screens to create and test responsive UIs on Android so that users love your app no matter what device they’re using it on.

### $\color{#499CD6}{Drag & Drop for Seamless Multitasking}$
With the increase in large screen devices, users are using multiple apps at the same time more and more. By adding support for dragging and dropping content from/into your app, you can cut out friction and delight your users with great cross-app interactions!

### $\color{#499CD6}{Why and How to Optimize Your App for ChromeOS}$
Millions of Android apps are available on ChromeOS today, and, if your app is in Google Play, it may be one of them. This talk will provide an overview of how we can take steps to ensure the best possible experience for our users on ChromeOS.

### $\color{#499CD6}{Adding Stylus Support to Your Android App}$
Learn how to optimize your Android apps for stylus input. By leveraging new Jetpack libraries, you can introduce immersive user experiences with stylus devices to achieve a similar experience to putting pen to paper.

### $\color{#499CD6}{Testing Wear OS Fitness Apps Without Breaking a Sweat}$
Compared to phone apps, developing high quality health and fitness experience for Wear—and especially performing manual QA—can be a bit tricky. Capabilities vary between devices, and running (or swimming!) over and over to test a user journey is not practical. In this lighting talk we go over a few options for testing health and fitness experiences with two feet firmly on the ground, via the emulator and health services' synthetic mode.

### $\color{#499CD6}{Around a Watch: Handling Rotary Input in Wear OS}$
Wear OS devices may contain a physical crown or a rotating bezel. When a user turns the crown, the system generates rotary events that developers can utilize to provide enhanced tactile interactions to the user. For example, this can be used for scrolling screens or to control audio volume. In this talk you will learn how to handle rotary input in your app.

### $\color{#499CD6}{Make Your App Shine for all Devices on Google Play}$
Your app listing information on Google Play is about to get more air time! Learn about new features in store and best practices for optimizing your app assets ahead of these changes.


## $\color{#499CD6}{平台 (Platform)}$
Tune in to learn about the latest updates to the Android platform.  
请关注以了解Android平台的最新更新。  
![平台](./2022%20Android%20Dev%20Summit/dcim/04.top3picks_03.png "平台")

### $\color{#499CD6}{Migrate Your Apps to Android 13}$
Each new release of Android comes with platform behavior changes that your app needs to account for; some of these changes apply only when you target the new SDK version, while others — mostly involving privacy and security — apply to all apps. We'll cover these changes, give insight on how to test your app, and talk about new Android 13 features you can take advantage of to give your Android 13 early adopters the best experience.

### $\color{#499CD6}{Presenting a High-quality Media Experience for all Users}$
Media experiences have a strong dependence on a variety of factors, like the device’s hardware capabilities and the properties of the media file itself, forming a complicated matrix of scenarios that developers need to handle. This talk will discuss tools and strategies for making sure your media app is optimized to present the best experience to all your users regardless of the use-case.

### $\color{#499CD6}{Improving Your Social Experience Quality with Android Camera}$
In this session, we will explore new framework innovations to improve quality, improve latency and create innovative experiences with Android Camera.

### $\color{#499CD6}{Building for a Multilingual World}$
Learn best practices to internationalize your android app and how to implement per-app language preferences.

### $\color{#499CD6}{Migrate to Play Billing Library 5}$
Google Play added new subscription features in May 2022 that enable more flexibility and complexity in your subscription product catalog. Learn how to adapt your Android and server integrations by migrating to Play Billing Library 5 and adopting the new endpoints created to take advantage of the new capabilities, and design your system to reduce the cost of maintenance.

### $\color{#499CD6}{Designing a High Quality App with the Latest Android Features}$
The recent releases of Android have brought a totally reimagined UI that feels alive with every tap, swipe and scroll. In this session, we will cover 3 platform features to help you polish your app with premium layouts, delightful navigation and an accessible color system. Users expect a high quality experience for their apps - how can developers meet that?

### $\color{#499CD6}{Hardware Acceleration for On-Device Machine Learning}$
Hardware acceleration can dramatically reduce inference latency for machine learning enabled features and allow you to deliver live on-device experiences that may not be possible otherwise. Today, in addition to CPU, Android devices embed various specialized chips such as GPU, DSP or NPU that you can use to accelerate your ML inference. In this talk we will go over some tools and solutions offered by TensorFlow and Android ML teams that help you take advantage of various hardware to accelerate ML inference in your Android app.

### $\color{#499CD6}{Demystifying Attestation}$
Device trust is complicated but essential for modern apps. Even the best mobile developers at the largest companies rarely have the time to become experts. In this talk - we will discuss what attestation is, which apps should be taking advantage of it, what actions you should take if you don’t trust a device, and how Play Integrity API simplifies your path toward improving your app secuirty.

### $\color{#499CD6}{Building Accessibility Support for Compose}$
Jetpack Compose is Android’s new toolkit for building native UI, and in this talk, we’ll talk about what it took to build a new UI toolkit to be compatible with accessibility services. This talk aims to help developers gain a deeper understanding of how various accessibility services, such as TalkBack and Switch Access, are able to understand and monitor the state of the UI in an Android app.

### $\color{#499CD6}{Supporting BLE Audio in Your Voice Communication Applications}$
Android 13 introduces support for BLE Audio hearables and within the next year hardware devices will be available in the market. This technical session will focus on how the Telecom API can support BLE Audio hearables to make use for high-quality bi-directional audio upto 32khz, stereo microphone support & many more features.

### $\color{#499CD6}{Next Up on the Privacy Sandbox}$
Overview of the Privacy Sandbox on Android including our plans for Beta and beyond. Learn about the new features in each Privacy Sandbox API and how to take advantage of them in your app or game.

### $\color{#499CD6}{Everything About Storage on Android}$
Persistence is a core element of every mobile app. Android provides different APIs to access or expose files with different tradeoffs. Should you request WRITE_EXTERNAL_STORAGE? How can you access an image on shared storage? In this session, you’ll be able to understand the key concepts of storage and take advantage of recent APIs to improve both your developer productivity and users' privacy.

### $\color{#499CD6}{HDR 10BIT: Capture, Playback, and Sharing 10BIT Video}$
This talk will take a dive into HDR video and discuss the process from end-to-end, including video capture, editing, playback, and sharing. Specific topics we can discuss include the new Media3 Transformer API for editing, displaying graphics on SurfaceView for playback, and any additional best practices to prepare the files for sharing.

### $\color{#499CD6}{Foster User Trust by Adopting Privacy-respecting Permission Workflows}$
In this talk, we’ll reiterate some big launches in Android permissions over the past few releases, while explaining why we believe in the set of principles above and showcase some of our own Google apps that we believe have adopted these in their own apps to build a better privacy experience for their users. We hope that we can inspire developers to adopt these best practices and enhance user trust in their app experiences.

### $\color{#499CD6}{Building Modern Android App Widgets}$
Does your app have an app widget or you want to build one? In this session we will showcase how we modernized our app widgets leading to more engagement and we will share top tips to help you build Modern Android AppWidgets

### $\color{#499CD6}{Keep your App From Failing in a 64-bit Only World}$
64-bit only opens up some gaps in the way people have been using the platform. This talk covers how to make sure your app will work on the next generation of Android devices.

### $\color{#499CD6}{Introduction to Ultrawide-band on Android}$
Introduction to Ultrawide-band technology, key concepts and real life applications. Also a walkthrough of our new Jetpack library to build apps for Android with it and sample code.

### $\color{#499CD6}{Syncing Data with Health Connect}$
Fitness apps store activities in a database. Health Connect also stores activities in a database (a different one!). How can activities be synced between the two (including propagating deletes and updates) in a way that is consistent, reliable, and comprehensible to the user?

### $\color{#499CD6}{Android Graphics}$
Come learn how to use Android's graphics APIs in your app, including taking advantage of some of the newest platform features such as AGSL, Android Graphics Shading Language.
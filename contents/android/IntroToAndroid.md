# Introduction to Android App Development

Authors: [Lee Yan Hwa](https://github.com/leeyh20)

- [Getting Started](#1-getting-started)
    - [What is Android?](#11-what-is-android)
    - [Develop your first app today!](#12-develop-your-first-app-today)

- [Android is not magic](#2-android-is-not-magic)
    - [App Components](#21-app-components)
    - [Activity Lifecycle](#22-activity-lifecycle)
    - [The Architecture of Android](#23-the-architecture-of-android)
    - [Resources](#24-resources)
    - [Testing](#25-testing)
- [Why should I make native Android apps?](#3-why-should-i-make-native-android-apps)
- [Further Readings](#4-further-readings)

## 1. Getting Started

### 1.1 What is Android?

Android is a Linux-based mobile phone operating system (OS), created by Android Inc. which was then acquired by Google. It was launched in 2008. Not only does it power phones, it is already the OS of watches, televisions and even car stereo systems.

The source code of Android is made available through the [Android Open Source Project](https://source.android.com/) (AOSP). The original AOSP code is used mainly on the Nexus and Pixel phones that are developed by Google itself. The AOSP code is customized and adapted by original equipment manufacturers (OEMs), such as Samsung and Sony, to run on their respective phones. There is a list of OEMs [here](https://www.android.com/certified/partners/). Android also has many versions, from Android Ice Cream Sandwich to the latest Android Oreo (as of March 2018). The Android ecosystem is quite [fragmented](https://www.androidauthority.com/android-version-distribution-748439/), with mainly 5 Android versions being installed in Android devices today.

But why should we develop apps for Android? This is because Android has become the [world's most popular mobile platform](https://developer.android.com/about/android.html). The popularity of Android cannot be denied. The Google Play Store currently has around 3 million apps as of 2017 ([source](http://www.businessofapps.com/data/app-statistics/)). It's a great time to step into the world of developing Android apps!

### 1.2 Develop your first app today!

The official programming language for Android development is Java, however, Android announced its support for Kotlin in 2017([source](https://android-developers.googleblog.com/2017/05/android-announces-support-for-kotlin.html)). Fret not, even if you are not an expert in either of those programming languages, you still can learn how to create your first app. Here's how.
- Start by downloading the latest version of [Android Studio](https://developer.android.com/studio/index.html), the official IDE for Android.
- Build your first app by going through the 'App Basics' tutorials at Google's Developer Guides [here](https://developer.android.com/training/basics/firstapp/index.html). If you prefer to watch a video instead of reading text, check out the [free Udacity course on Android Fundamentals](https://www.udacity.com/course/new-android-fundamentals--ud851), which is the first course of their [Android Developer Nanodegree](https://www.udacity.com/course/android-developer-nanodegree-by-google--nd801).
- For more experienced programmers, popular Android development courses include those by Stanford at [their Android App Development module page](http://web.stanford.edu/class/cs193a/) and [Udemy's paid courses](https://www.udemy.com/courses/search/?q=android&src=ukw). A good book for Android would be the [Android Programming: The Big Nerd Ranch Guide](https://www.bignerdranch.com/books/android-programming/).
- To learn Android development at your own pace, check out the comprehensive [Vogella Android tutorials](http://www.vogella.com/tutorials/android.html), which cover almost everything you would need to know. There are also open-source crowdsourced [guides by CodePath](https://guides.codepath.com/android).

## 2. Android is not magic

### 2.1 App Components

App components are building blocks of an Android app. You can easily build an Android app with App components, even if you are not an UI/UX master!
Some app component examples include:

#### Activities
An `Activity` represents a single screen with a user interface. For example, an event app could have an `Activity` to login, an `Activity` to view the event schedule and another `Activity` to search for locations.

#### Layouts
`Layouts` defines a user interface structure for an `Activity`. They specify how each child   `View` will be placed. For example, [ConstraintLayout](https://developer.android.com/training/constraint-layout/index.html) allows you to define constraints between views that will lead to a more responsive UI. Often, apps also need to display a scrolling list of elements and this is where [RecyclerView](https://developer.android.com/guide/topics/ui/layout/recyclerview.html) comes in handy. The `RecyclerView` is very versatile. It can be used to easily create apps like the one below:

![RecyclerView Example](https://i.imgur.com/vbIL5HA.gif)

([Image](https://i.imgur.com/vbIL5HA.gif) from [Codepath](https://guides.codepath.com/android/using-the-recyclerview))

It can also be used to create apps that will benefit from a Grid or Staggered Grid. For example, to have a Staggered Grid, you just need a few lines of Java code as seen below:
```Java
// First parameter is number of columns and second param is orientation i.e Vertical or Horizontal
StaggeredGridLayoutManager gridLayoutManager = new StaggeredGridLayoutManager(2, StaggeredGridLayoutManager.VERTICAL);
// Attach the layout manager to the recycler view
recyclerView.setLayoutManager(gridLayoutManager);
```

Sample code is from [Codepath](https://guides.codepath.com/android/using-the-recyclerview).

The app with a Staggered Grid will then look like this:

![Staggered Grid Example](https://i.imgur.com/AlANFgj.png)

([Image](https://i.imgur.com/AlANFgj.png) from [Codepath](https://guides.codepath.com/android/using-the-recyclerview))

You can also build a `Layout` easily using [Android Layout Editor](https://developer.android.com/studio/write/layout-editor.html).
For further information, Google I/O 2016 introduces `Layouts` in [this video](https://www.youtube.com/watch?v=sO9aX87hq9c&t=207s).

#### Fragments
`Fragments` are like 'sub activities' that allow for code reuse in different activities. They are modular sections with their own lifecycles that "sit" on top of an `Activity`, breaking down a huge `Activity` into smaller components. For example, a scrolling list on the left of a `Activity` can be a `Fragment` and a display box on the right of the same `Activity` can be another `Fragment`. Tablets and handsets can choose to display these fragments differently due to the difference in screen size, thus with Fragments, you can easily cater for mobile devices with different resolutions!

![Fragments Example](https://camo.githubusercontent.com/b768afff0888fcb8cbe1704b0609b53110276969/687474703a2f2f646576656c6f7065722e616e64726f69642e636f6d2f696d616765732f66756e64616d656e74616c732f667261676d656e74732e706e67)

(Image from [Codepath](https://github.com/codepath/android_guides/wiki/Creating-and-Using-Fragments))

Learn more about `Fragments` in [this video from Google I/O 2016](https://www.youtube.com/watch?v=k3IT-IJ0J98&t=618s) which tries to clarify when a `Fragment` should be used over a `View`.


### 2.2 Activity Lifecycle

You might wonder, how can I deal with scenarios where the user switch to another app? What if they rotate their phone? Don't worry, you will have control over your app as the user interacts with your app differently. Control is given to you using the Activity Lifecycle.

![Activity Lifecycle](https://developer.android.com/guide/components/images/activity_lifecycle.png)

([Image](https://developer.android.com/guide/components/images/activity_lifecycle.png) from Google)

Activity instances in an app will transition through different stages in their `lifecycle`. An activity that has just been created will be in the `Created` stage. When the app prepares for the activity to enter the foreground and be displayed to the user, it enters the `Started` stage. The UI could be initialised at this stage. When the activity comes to the foreground and can be viewed by the user, it enters the `Resumed` stage. More details about the other stages can be found in the picture above or at the [Activity Lifecycle](https://developer.android.com/guide/components/activities/activity-lifecycle.html) page. Knowing about the `Activity Lifecycle` will ensure that you can save the user's progress in your app as they switch between apps or rotate between landscape and portrait orientations. You can also easily prevent your app from using precious system resources when the user is not currently using your app.

The `Activity` class implements callbacks that will be invoked as the activity enters a new state. These callbacks can be overridden in your activity class that inherits from `Activity` so that your app can respond to transitions between states.

### 2.3 The Architecture of Android

Developing for Android used to be a daunting task as apps needed to be reactive to data changes, responsive to users and yet deal with the entire Activity Lifecycle. At Google I/O 2017, [Architecture Components](https://developer.android.com/topic/libraries/architecture/index.html) were introduced to help developers solve common issues by following best practices and implementing recommended architecture, so you can focus on building better apps!

#### LiveData
Some examples of useful components for developers include the `LiveData` observable data holder class. Now with `LiveData`, we are able to create observables (Read more about the Observable-Pattern [here](https://code.tutsplus.com/tutorials/android-design-patterns-the-observer-pattern--cms-28963)) that are `lifecycle-aware`, only updating app components that are active. This prevents memory leaks and crashes. For more information, see the [LiveData page](https://developer.android.com/topic/libraries/architecture/livedata.html).

#### ViewModel
Another very useful component is the `ViewModel` component. It manages and store UI data in a `lifecycle-aware` way, allowing data to be retained during configuration changes such as screen rotations. Using normal UI controllers require data to be saved and then restored by overridding `onSaveInstanceState` or by using `SharedPreferences`, but this is inefficient for large amounts of data. For more information, see the [ViewModel page](https://developer.android.com/topic/libraries/architecture/viewmodel.html).

#### How does this link to having better architecture?
I suppose you are a Software Engineer, or at least, aspiring to be one. You would probably have heard about good architectural patterns in a Software Engineering module. Thus you might be glad to hear that there are common architectural patterns used in Android app development so that we can easily structure our app.

Previously, Android apps were usually developed using the `Model-View-Presenter` (MVP) pattern or the popular Model-View-Controller (MVC) pattern. However, this will change with the above `LiveData` and `ViewModel` components. Now, we can create apps that follow the `Model-View-ViewModel` (MVVM) architecture instead. The `ViewModel` component provided by Android serves as a good base for our `ViewModel` in the MVVM architecture.

To read more about the MVVM architecture in Android apps, see this [tutorial](https://proandroiddev.com/mvvm-architecture-viewmodel-and-livedata-part-1-604f50cda1). For a comparison of the three patterns, see [this article at Realm's Academy](https://academy.realm.io/posts/eric-maxwell-mvc-mvp-and-mvvm-on-android/).

### 2.4 Resources

Apps rely a lot on external resources and Android provides an easy way of accessing these resources.

Android manages external resources in one central place. Any external resources that the app requires such as images, XML layout files, fonts, values such as strings and colors should be placed in the `res/` directory of the Android project. The resource then can be accessed by referencing its resource ID. Its resource ID is composed of the [resource type](https://developer.android.com/guide/topics/resources/available-resources.html) and the resource name. `R.drawable.myimage` is an example of a way to access an image resource in code. `@string/submit` is an example of a way to access a string value from XML. Read more about accessing resources [here](https://developer.android.com/guide/topics/resources/accessing-resources.html).

### 2.5 Testing

Testing used to be a nightmare for app development but since 2017, Test-driven development was [encouraged](https://www.youtube.com/watch?v=pK7W5npkhho) by Google in their Google I/O 2017 due to the release of the `Android Testing Support Library`. You can read more about the library [here](https://developer.android.com/topic/libraries/testing-support-library/index.html). Now that we can do testing easily, we can build more reliable apps!

#### Unit Testing and Mocking
Mocking is usually needed in unit testing to simulate the behaviour of real objects (that are dependencies) to verify the behaviour of the object that you are testing. In Android app development, [Mockito](http://site.mockito.org/), a popular mocking library for unit tests in Java, is often used. However, due to the difficulty of mocking the Android SDK to create unit tests, [Robolectric](http://robolectric.org/) was developed to solve this issue. Robolectric can still be used alongside Mockito. The benefit of Robolectric is that it handles emulation UI code such that tests that rely on the UI can be run on the Java Virtual Machine (JVM) rather than having to run them on an emulator. In other words, tests can be written and run faster.

#### Integration Testing
Integration testing and end-to-end testing is usually done using [Espresso](https://developer.android.com/training/testing/espresso/index.html). You can even record your own UI tests using the [Espresso Test Recorder](https://developer.android.com/studio/test/espresso-test-recorder.html) to save time! Be sure to check out the Espresso [cheatsheet](https://developer.android.com/training/testing/espresso/cheat-sheet.html) for easy writing of tests.

## 4. Why should I make native Android apps?

As of year 2018, there are many ways to develop an app. Native apps are those created specifically for the mobile operating system. For Android, it means that you will use Java or Kotlin with the Android SDK and Google's official tools like Android Studio, just like how it is detailed in this book chapter.

So you might then consider developing a web app instead, or even a [progressive web app](https://developers.google.com/web/progressive-web-apps/) which are loaded in browsers like Chrome and Firefox, using Javascript, CSS and HTML. This means that the user does not need to download your app and your app can directly be accessed from the web. However, web apps often use the same user interface for both Apple and Android phones and this might not feel integrated with the rest of the phone because Apple and Android apps usually use [different interaction design patterns](https://medium.com/@vedantha/interaction-design-patterns-ios-vs-android-111055f8a9b7). Additionally, web apps often suffer from the lack of functionality. Thus web apps will not fully replace the need for native Android apps.

Then, there are [hybrid apps](https://blog.techmagic.co/native-vs-hybrid-apps/). Creating a hybrid app using hybrid mobile frameworks such as Ionic and React Native allow us to develop for both Android and Apple at the same time (just like web apps), while allowing us to access device-only features such as the camera or GPS. Indeed, a cross-platform approach is appealing for people looking to quickly develop an app. However, you might end up using more time trying to tweak the app to improve its performance and UI to suit both platforms. Developing a native app is still the best way to ensure performance, security, a responsive and integrated user interface and access to native APIs.

Until hybrid mobile frameworks catch up with native apps and can fully replace native development, there are often more benefits to developing native apps.

## 5. Further Readings

Going further, it will be useful to know about:
*   [Firebase](https://firebase.google.com/), a mobile platform provided by Google that provides various services to grow and monetise your app.
*   [Reactive Programming with RxJava](https://blog.mindorks.com/rxjava-anatomy-what-is-rxjava-how-rxjava-is-designed-and-how-rxjava-works-d357b3aca586).
*   [RxJava](https://medium.com/@kevalpatel2106/code-your-next-android-app-using-rxjava-d1db30ac9fcc), a Java VM implementation of ReactiveX (Reactive Extensions): a library for composing asynchronous and event-based programs by using observable sequences.
*   [Realm](https://realm.io/), a NoSQL object-orientated mobile database system.
*   [Android Debug Bridge](https://developer.android.com/studio/command-line/adb.html), a powerful command line tool for Android.
*   [Advanced Android Expresso](https://academy.realm.io/posts/chiu-ki-chan-advanced-android-espresso-testing/), the automated testing tool for Android UI tests.

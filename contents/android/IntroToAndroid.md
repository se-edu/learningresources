# Introduction to Android App Development

Authors: [Lee Yan Hwa](https://github.com/leeyh20)

- [Getting Started](#-1-getting-started)
    - [What is Android?](#-11-what-is-android)
    - [Develop your first app today!](#-12-develop-your-first-app-today)

- [Understanding Android](#-2-getting-started)
    - [App Components](#-22-app-components)
    - [Activity Lifecycle](#-23-activity-lifecycle)
    - [MVVM and Architecture Components](#-24-mvvm-and-architecture-components)
    - [Resources](#-25-resources)
    - [Testing](#-26-testing)
- [Further Readings](#-3-further-readings)

## 1. Getting Started

### 1.1 What is Android?
Android is a Linux-based mobile phone operating system (OS), created by Android Inc. which was then acquired by Google. It was launched in 2008. Not only does it power phones, it is already the OS of watches, televisions and even car stereo systems.

The source code of Android is made available through the [Android Open Source Project](https://source.android.com/) (AOSP). The original AOSP code is used mainly on the Nexus and Pixel phones that are developed by Google itself. The AOSP code is customized and adapted by original equipment manufacturers (OEMs), such as Samsung and Sony, to run on their respective phones. There is a list of OEMs [here](https://www.android.com/certified/partners/). Android also has many versions, from Android Ice Cream Sandwich to the latest Android Oreo (as of March 2018). The Android ecosystem is quite [fragmented](https://www.androidauthority.com/android-version-distribution-748439/), with mainly 5 Android versions being installed in Android devices today.

The popularity of Android cannot be denied. Google Play Store currently has around 3 million apps as of 2017 ([source](http://www.businessofapps.com/data/app-statistics/)), while Android has become the [world's most popular mobile platform](https://developer.android.com/about/android.html). It's a great time to step into the world of developing Android apps!

### 1.2 Develop your first app today!

The official programming language for Android development is Java, however, Android announced its support for Kotlin in 2017.([source](https://android-developers.googleblog.com/2017/05/android-announces-support-for-kotlin.html)) Fret not, even if you are not an expert in either of those programming languages, you still can learn how to create your first app. Here's how.
- Start by downloading the latest version of [Android Studio](https://developer.android.com/studio/index.html), the official IDE for Android.
- Build your first app by going through the 'App Basics' tutorials at Google's Developer Guides [here](https://developer.android.com/training/basics/firstapp/index.html). If you prefer to watch a video instead of reading text, check out the [free Udacity course on Android Fundamentals](https://www.udacity.com/course/new-android-fundamentals--ud851), which is the first course of their [Android Developer Nanodegree](https://www.udacity.com/course/android-developer-nanodegree-by-google--nd801).
- For more experienced programmers, popular Android development courses include those by Stanford at [their Android App Development module page](https://hackr.io/tutorial/android-app-development-by-stanford) and [Udemy's paid courses](https://www.udemy.com/courses/search/?q=android&src=ukw). A good book for Android would be the [Android Programming: The Big Nerd Ranch Guide](https://www.bignerdranch.com/books/android-programming/).
- To learn Android development at your own pace, check out the comprehensive [Vogella Android tutorials](http://www.vogella.com/tutorials/android.html), which cover almost everything you would need to know. There are also open-source crowdsourced [guides by CodePath](https://guides.codepath.com/android).

## 2. Understanding Android

### 2.2 App Components

App components are building blocks of an Android app.

#### Activities
An `Activity` represents a single screen with a user interface. For example, an event app could have an `Activity` to login, an `Activity` to view the event schedule and another `Activity` to search for locations.

#### Layouts
`Layouts` defines a user interface structure for an `Activity`. They specify how each child   `View` will be placed. For example, [ConstraintLayout](https://developer.android.com/training/constraint-layout/index.html) allows you to define constraints between views that will lead to a more responsive UI. Often, apps also need to display a scrolling list of elements and this is where [RecyclerView](https://developer.android.com/guide/topics/ui/layout/recyclerview.html) comes in handy. You can build a `Layout` easily using [Android Layout Editor](https://developer.android.com/studio/write/layout-editor.html).
For further information, Google I/O 2016 introduces `Layouts` in [this video](https://www.youtube.com/watch?v=sO9aX87hq9c&t=207s).

#### Fragments
`Fragments` are like 'sub activities' that allow for code reuse in different activities. They are modular sections with their own lifecycles that "sit" on top of an `Activity`, breaking down a huge `Activity` into smaller components. For example, a scrolling list on the left of a `Activity` can be a `Fragment` and a display box on the right of the same `Activity` can be another `Fragment`. Tablets and handsets can choose to display these fragments differently due to the difference in screen size. Learn more about `Fragments` in [this video from Google I/O 2016](https://www.youtube.com/watch?v=k3IT-IJ0J98&t=618s) which tries to clarify when a `Fragment` should be used over a `View`.

#### Services
A `Service` performs long-running operations in the foreground or background, and it does not provide a user interface. For example, a `Service` can play audio in the foreground or download data in the background. Read more at the [Services](https://developer.android.com/guide/components/services.html) page in the Android Developer Guides.

#### Broadcast receivers
A `Broadcast Receiver` allows the app to register for events sent by the System, events sent by another application or events sent within the same application. Even if the app is inactive, the app is able to respond to system-wide broadcast announcements. The system automatically sends broadcasts such as when the system switches in and out of airplane mode. Read more at the [Broadcasts](https://developer.android.com/guide/components/broadcasts.html) page.

#### Content providers
A `Content provider` abstracts the data layer, encapsulating data and allowing for data security. Read more at the [Content providers](https://developer.android.com/guide/topics/providers/content-providers.html) page.

### 2.3 Activity Lifecycle
![Activity Lifecycle](https://developer.android.com/guide/components/images/activity_lifecycle.png)

([Image](https://developer.android.com/guide/components/images/activity_lifecycle.png) from Google)

Activity instances in an app will transition through different stages in their `lifecycle`. An activity that has just been created will be in the `Created` stage. When the app prepares for the activity to enter the foreground and be displayed to the user, it enters the `Started` stage. The UI could be initialised at this stage. When the activity comes to the foreground and can be viewed by the user, it enters the `Resumed` stage. More details about the other stages can be found in the picture above or at the [Activity Lifecycle](https://developer.android.com/guide/components/activities/activity-lifecycle.html) page. Knowing about the `Activity Lifecycle` will ensure that you can save the user's progress in your app as they switch between apps or rotate between landscape and portrait orientations. You also can prevent your app from using precious system resources when the user is not currently using your app.

The `Activity` class implements callbacks that will be invoked as the activity enters a new state. These callbacks can be overridden in your activity class that inherits from `Activity` so that your app can respond to transitions between states.

The following code is an example of overriding the `onCreate` callback. It calls the superclass method first and then declares the user interface which is defined in an XML layout file `R.layout.activity_main`.

```java
public class MainActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
```
To free up RAM, the system might kill the process in which an activity runs and the likelihood of this happening depends on the state of the process. It is more likely for the process to be killed when the process is empty or in the background and when the activity is stopped or destroyed.
For more information, see the table [here](https://developer.android.com/guide/components/activities/activity-lifecycle.html#asem).

### 2.4 MVVM and Architecture Components
At Google I/O 2017, [Architecture Components](https://developer.android.com/topic/libraries/architecture/index.html) were introduced to help developers solve common issues by following best practices and implementing recommended architecture.

#### LiveData
Some examples of useful components for developers include the `LiveData` observable data holder class. Now with `LiveData`, we are able to create observables (Read more about the Observable-Pattern [here](https://code.tutsplus.com/tutorials/android-design-patterns-the-observer-pattern--cms-28963)) that are `lifecycle-aware`, only updating app components that are active. This prevents memory leaks and crashes. For more information, see the [LiveData page](https://developer.android.com/topic/libraries/architecture/livedata.html).

#### ViewModel
Another very useful component is the `ViewModel` component. It manages and store UI data in a `lifecycle-aware` way, allowing data to be retained during configuration changes such as screen rotations. Using normal UI controllers require data to be saved and then restored by overridding `onSaveInstanceState` or by using `SharedPreferences`, but this is inefficient for large amounts of data. For more information, see the [ViewModel page](https://developer.android.com/topic/libraries/architecture/viewmodel.html).

#### How does this link to MVVM?
Previously, Android apps were usually developed using the `Model-View-Presenter` (MVP) pattern or the popular Model-View-Controller (MVC) pattern. However, this will change with the above `LiveData` and `ViewModel` components. Now, we can create apps that follow the `Model-View-ViewModel` (MVVM) architecture instead. The `ViewModel` component provided by Android serves as a good base for our `ViewModel` in the MVVM architecture. Our `ViewModel` in MVVM will interact with the Model and manages and store data to be observed by a View. The `ViewModel` is not aware about the View that is getting data from it to update the UI and thus it is decoupled from the View. This is very unlike the Presenter in the MVP pattern, which tells the View what to display through an interface. The Controller in the MVC pattern directly tells the Views how to show the data and is tightly coupled to the Views. To read more about the MVVM architecture in Android apps, see this [tutorial](https://proandroiddev.com/mvvm-architecture-viewmodel-and-livedata-part-1-604f50cda1). For a comparison of the three patterns, see [this article at Realm's Academy](https://academy.realm.io/posts/eric-maxwell-mvc-mvp-and-mvvm-on-android/).

### 2.6 Testing
Test-driven development was [encouraged](https://www.youtube.com/watch?v=pK7W5npkhho) by Google in their Google I/O 2017 due to the release of the `Android Testing Support Library`. You can read more about the library [here](https://developer.android.com/topic/libraries/testing-support-library/index.html).

#### Unit Testing and Mocking
Mocking is usually needed in unit testing to simulate the behaviour of real objects (that are dependencies) to verify the behaviour of the object that you are testing. In Android app development, [Mockito](http://site.mockito.org/), a popular mocking library for unit tests in Java, is often used. However, due to the difficulty of mocking the Android SDK to create unit tests, [Robolectric](http://robolectric.org/) was developed to solve this issue. Robolectric can still be used alongside Mockito. The benefit of Robolectric is that it handles emulation UI code such that tests that rely on the UI can be run on the Java Virtual Machine (JVM) rather than having to run them on an emulator. In other words, tests can be written and run faster.

#### Integration Testing
Integration testing and end-to-end testing is usually done using [Espresso](https://developer.android.com/training/testing/espresso/index.html). You can even record your own UI tests using the [Espresso Test Recorder](https://developer.android.com/studio/test/espresso-test-recorder.html) to save time! Be sure to check out the Espresso [cheatsheet](https://developer.android.com/training/testing/espresso/cheat-sheet.html) for easy writing of tests.


## 3. Further Readings
Going further, it will be useful to know about:
*   [Firebase](https://firebase.google.com/), a mobile platform provided by Google that provides various services to grow and monetise your app.
*   [Reactive Programming with RxJava](https://blog.mindorks.com/rxjava-anatomy-what-is-rxjava-how-rxjava-is-designed-and-how-rxjava-works-d357b3aca586).
*   [RxJava](https://medium.com/@kevalpatel2106/code-your-next-android-app-using-rxjava-d1db30ac9fcc), a Java VM implementation of ReactiveX (Reactive Extensions): a library for composing asynchronous and event-based programs by using observable sequences.
*   [Realm](https://realm.io/), a NoSQL object-orientated mobile database system.
*   [Android Debug Bridge](https://developer.android.com/studio/command-line/adb.html), a powerful command line tool for Android.
*   [Advanced Android Expresso](https://academy.realm.io/posts/chiu-ki-chan-advanced-android-espresso-testing/), the automated testing tool for Android UI tests.

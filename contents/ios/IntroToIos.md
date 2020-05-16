<frontmatter>
  title: Introduction to iOS App Development
  header: pagetop.md
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

<div class="website-content">

{{ booktitle | safe }}

# Introduction to iOS Development

**Authors: [Bryan Lew](https://github.com/blewjy)**

Reviewers: [Chester Sng](https://github.com/ChesterSng), [Jiang Chunhui](https://github.com/Adoby7), [Yu Pei, Henry](https://github.com/YuPeiHenry)

<box id="article-toc">

* [What is iOS?‎](#what-is-ios)
* [Why iOS?‎](#why-ios)
* [Native iOS Applications vs. Cross-Platform Applications‎](#native-ios-applications-vs-cross-platform-applications)
* [Getting Started With Native iOS Development‎](#getting-started-with-native-ios-development)
</box>

## What is iOS?

iOS is the mobile operating system that runs on Apple's mobile devices, most notably the iPhone and the iPad. Applications that run on iOS can be downloaded officially from the App Store, and developers of iOS applications can submit their own applications to the App Store to share it with the rest of the world. 

## Why iOS?

[2 operating systems dominate the whole market share: iOS and Android](https://www.theverge.com/2017/2/16/14634656/android-ios-market-share-blackberry-2016). While more devices are running Android compared to iOS, given below are some areas in which iOS outshines Android:

#### Better Compatibility and Standardisation

iOS applications are specific to the Apple devices of the iPhone and iPad line, whereas Android runs across numerous different types of devices, including phones, tablets, watches, and more. The large variation of devices poses a compatibility problem from a developer's standpoint, because now applications have to be tested and working across all these different devices, and also regularly maintained this way. Each hardware device may possibly be running their own version of Android or will only support up to a particular version, thus Android developers will have to ensure that their application is also compatible across different versions of Android.

With less devices to target, iOS developers have more control over their application and are able to customise the application to a greater extent, because there are less variations (in terms of device compatibility) to worry about. OS versioning in iOS is also more structured, with better backwards-compatability across their devices. Users of iOS devices (oldest supported is iPhone 5S, as of March 2019) are always immediately prompted whenever a new version of iOS is available for download. This means that as developers, we have to worry less about compatability issues, and focus more on the application itself.

#### Lucrative Market

While Android has higher market share of devices, iOS applications generate [nearly double](https://techcrunch.com/2018/07/16/apples-app-store-revenue-nearly-double-that-of-google-play-in-first-half-of-2018/) the revenue compared to Android, largely due to the fact that users of iOS devices are generally more likely to spend on apps.

As an independent iOS developer, this means that your iOS application could bring in more revenue for you as compared to an Android one, assuming you choose to monetize it. 

More app revenue for iOS applications also means companies will look to tackle the iOS platform more so than Android, thus the demand for iOS developers will be high. This in turn results in [higher salary for iOS developers compared to their Android counterparts](https://www.fiercewireless.com/developer/ios-developers-earn-roughly-10k-more-than-android-counterparts-study-shows).

#### Higher Quality Applications

The Apple App Store [subjects the apps to higher quality control](https://developer.apple.com/app-store/review/) (as compared to Android Play Store) before they are allowed to be published. This means that applications on the App Store are more robust, and of higher quality in general.

## Native iOS Applications vs. Cross-Platform Applications

Mobile applications that run on the iOS platform can be written both natively and using [cross-platform solutions](https://www.businessofapps.com/guide/cross-platform-mobile-app-development/). Native iOS applications are written using [Objective-C or Swift](https://android.jlelse.eu/objective-c-or-swift-which-technology-to-learn-for-ios-app-development-3c681d1a05ac) on the Xcode IDE that you can download if you are running a MacOS. Cross-platform solutions are tools that allow you to write code once and develop applications for more than 1 platform. Some examples include [React Native](https://facebook.github.io/react-native/), [Xamarin](https://visualstudio.microsoft.com/xamarin/), and [Ionic](https://ionicframework.com/). Both methods have their own pros and cons, and these factors not only affect the app developers, but also the users to some extent.

The main attraction to developing mobile applications using cross-platform solutions is development time. Cross-platform solutions allow you to write code once, but push it out to more than one OS, usually iOS and Android being two of these platforms. This means that essentially, your development time is cut by half, because you are only writing code once for 2 separate applications. While this may sound like an attractive deal, there are a bunch of caveats to consider that can potentially be a deal breaker, [performance and compatability issues being the frontrunners](https://codeburst.io/native-vs-cross-platform-app-development-pros-and-cons-49f397bb38ac).

If you are coming from a web development background, you might want to consider starting with React Native. React Native as a cross-platform mobile development framework will give you a mobile development environment that is very similar to a web development one (especially if you are familiar with ReactJS), using the same JavaScript structure as ReactJS, and also uses `props`, `state`, and all the standard React component lifecycle methods. It will allow you to learn about mobile development while on familiar ground.

If you are a complete beginner to programming or do not have much software engineering experience, native iOS development with Swift might be a better choice to start with. Given that Swift is a statically typed language, more errors will be caught earlier, and you will be forced to be more structured and disciplined in your code. With Swift being a fast and readable language, it is not a bad language to start learning programming with or to pick up general software engineering skills with.

## Getting Started With Native iOS Development

To get started with native iOS development, we have to first get some of the basic tools set up:

- To use Xcode and write native iOS applications in Swift, you will need MacOS. If you are using a non-Mac operating system (Windows, Linux or others), you can install a MacOS Virtual Machine on your computer. This [blog post](https://medium.com/@twister.mr/installing-macos-to-virtualbox-1fcc5cf22801) is an excellent tutorial on how this can be done. If you already own a Macbook or an iMac, you are all set for this step.
- Next, you should register for a free Apple Developer Account. Don't confuse this with the paid iOS Developer Program! Anyone can register for the Apple Developer Account for free. Go to [Apple's Developer Website](https://developer.apple.com/register/) to do this. It requires you to have an Apple ID, so you can use your existing one if you already have an Apple ID.
- Once you have an Apple Developer Account, you can either directly download Xcode from the [website](https://developer.apple.com/xcode/), or search for Xcode on your Mac App Store. Xcode is the Integrated Development Environment (IDE) that provides you with everything you need to develop an iOS app from scratch. It also comes with the iPhone and iPad simulator that you will need to test your application.

![Xcode and Simulator](https://insights.dice.com/wp-content/uploads/2018/06/Xcode-Mac-iPad-Apple-Dice.png)
([Image](https://insights.dice.com/wp-content/uploads/2018/06/Xcode-Mac-iPad-Apple-Dice.png) from [Dice Insights](https://insights.dice.com/))

Native iOS applications can be written in [Objective-C or Swift](https://android.jlelse.eu/objective-c-or-swift-which-technology-to-learn-for-ios-app-development-3c681d1a05ac). Swift is a relatively newer language, introduced only in 2014, while Objective-C is more of an old school programming language. If you are just starting out on iOS development, you should strongly consider using Swift. The main reason being that many of the documentations and help on the internet are written for iOS development in Swift, hence it will be easier to look for resources that target Swift instead of Objective-C. Furthermore, Apple mostly regards Swift as the main language for iOS programming now, and Objective-C support is now more of a backward-compatability. 

If you are new to Swift, you may want to read up on our Swift article: [Introduction to Swift]({{baseUrl}}/contents/swift/welcome-to-swift.html)

If you are ready to begin developing your first iOS application, you will want to check out these iOS development tutorials and articles meant for first-timers:
- [Codewithchris: How to make an iPhone app](https://codewithchris.com/how-to-make-an-iphone-app/)
- [raywenderlich.com "How to build a simple iOS app"](https://www.raywenderlich.com/3114-ios-tutorial-how-to-create-a-simple-iphone-app-part-1-3)

Some useful iOS development resources:
- [Ray Wenderlich](https://www.raywenderlich.com/ios)
- [Brian Advent](https://www.youtube.com/channel/UCysEngjfeIYapEER9K8aikw)
- [Let's Build That App](https://www.youtube.com/channel/UCuP2vJ6kRutQBfRmdcI92mA)

</div>

<frontmatter>
  title: Introduction to iOS App Development
  footer: footer.md
  head: head.md
  siteNav: mainNav.md
  pageNav: 3
</frontmatter>

{{ navbar | safe }}

<div class="website-content">

# Introduction to iOS Development

Authors: [Bryan Lew](https://github.com/blewjy)

## 1. Overview

iOS is the mobile operating system that runs on Apple's mobile devices, most notably the iPhone and the iPad. Programs or applications that run on iOS can be downloaded officially from the App Store, and developers of iOS applications can submit their own applications to the App Store to share it with the rest of the world. 

iOS applications can be developed both natively and using [cross-platform solutions](http://www.businessofapps.com/guide/cross-platform-mobile-app-development/). In this article, we will only be diving into native iOS app development, which uses Xcode as its primary development environment. Native iOS apps can be written in both Objective-C and Swift, with the latter being the newer and largely preferred choice. We will be exploring iOS development in Swift 4. Check out this [blog post](https://android.jlelse.eu/objective-c-or-swift-which-technology-to-learn-for-ios-app-development-3c681d1a05ac) for more information about Swift vs. Objective-C.

## 2. Getting started

To get started with iOS development, we have to first get some of the basic tools set up:

- To use Xcode and write native iOS apps in Swift, you will need MacOS. If you are using a non-Mac operating system (Windows, Linus or others), you can install a MacOS Virtual Machine on your computer. This [blog post](https://medium.com/@twister.mr/installing-macos-to-virtualbox-1fcc5cf22801) is an excellent tutorial on how this can be done. If you already own a Macbook or a Mac desktop, you are all set for this step.
- Next, you should register for a free Apple Developer Account. Don't confuse this with the paid iOS Developer Program! Anyone can register for the Apple Developer Account for free. Go to [Apple's Developer Website](https://developer.apple.com/register/) to do this. It requires you to have an Apple ID, so you can use your existing one if you already have an Apple ID.
- Once you have an Apple Developer Account, you can either directly download Xcode from the [website](https://developer.apple.com/xcode/), or search for Xcode on your Mac App Store. Xcode is the Integrated Development Environment (IDE) that provides you with everything you need to develop an iOS app from scratch. It also somes with the iPhone and iPad simulator that you will need to test your application.

Once you have Xcode downloaded on your machine, you are all set and ready to begin developing your first iOS app!

## 3. Building the app

If you have decided to work with Swift and are new to the language, you can start by [reading the chapter on Swift]({{baseUrl}}/contents/swift/welcome-to-swift.html) right on this website.

Here are some explanations to some of the more commonly used components of iOS app developement that will make the process of developing your first iOS application much smoother.

#### UIKit

`UIKit` is the main framework provided by Apple that you can use to create the user interface (UI) for your iOS application, created and designed exclusively for iOS development. `UIKit` defines classes, protocols and functions that aids the developer in creating different types of views (called `UIView`) that you can place in your app's UI. `UIKit` embraces the Model-View-Controller (MVC) design pattern, and Apple also recommends this for iOS applications. In short, for MVC in iOS development:

- **Model** is where your app's data reside. This is typically represented by a `struct` or a `class`.
- **View** is what users of your application see. They are typically reusable, and should not contain any execution logic. In `UIKit`, you will come across many different types of views. Some examples include `UILabel`, `UIImageView`, `UICollectionView`, etc.
- **Controller** is typically the middle-man between the View and Model. You will encounter this with the `UIViewController` class, or the `UICollectionViewController` class in `UIKit`.

You can learn more about MVC with `UIKit` from [this blog post](https://www.raywenderlich.com/1073-model-view-controller-mvc-in-ios-a-modern-approach).

#### Auto Layout

Auto layout, in short, is a system of contraints and anchors built in a layout system that governs the size and position of views in the UI. We can think about it this way: Imagine you wanted to place a `UIImageView` in the center of your iPhone X screen, and you do so by specifying the exact coordinates of the center of the iPhone X screen to your `UIImageView`. While this works for this particular case, you will quickly realise that by simply rotating the device, or when rendering out the same view on devices of different sizes (like the iPhone XS Max, or iPad), the image does not render out in the center of the screen like you wanted. 

Auto layout solves this by allowing you to specify constraints or anchors to your views. As developers, we just have to tell each of the views how it should be placed **relative** to the screen or **relative** to the other views, and auto layout will take care of itself and adapt all the views according to the device’s screen size and dimensions. This is done by attaching (or "hooking") the different anchor points of each `UIView` to another `UIView`, or the screen itself. There are a total of 12 different anchors for each `UIView`, but to get started, the main ones that you will encounter most often are the following:

- `NSLayoutXAxisAnchor`: Left Anchor, Right Anchor, Center-X Anchor
- `NSLayoutYAxisAnchor`: Top Anchor, Bottom Anchor, Center-Y Anchor
- `NSLayoutDimension`: Height Anchor, Width Anchor

[Here is an informative blog post on auto layout](https://www.raywenderlich.com/443-auto-layout-tutorial-in-ios-11-getting-started).

#### Simulator

The simulator is the iOS emulator software that comes packaged with a default Xcode installation. It allows you to run your application direcly on your Mac, without having to have an acutal iOS device. As of Xcode 10, the simulator is able to emulate most of the iOS devices, including various versions of iPhone (iPhone 5S - iPhone XS Max) and iPad (5th generation - iPad Pro).

#### Storyboard

A storyboard is a visual representation of the user interface (UI) of an iOS application. From the storyboard files, you will be able to see exactly how the views of the UI are being placed on the screen, and you can compare the UI across screens of difference devices very quickly in your storyboard file. You can also drag and insert new views into the storyboard directly, and change the attributes of that view directly within storyboards, including changing the dimensions, positions, and even settings constraints and anchors for auto layout.

In essence, storyboards allow you to design the UI of your application graphically instead of using code, and also gives you a visual representation of how your app's UI might look like without having to build and run the application.

Storyboard files are appended with the `.storyboard` extension.

Once you are familiar with all these, building your first iOS application should be a breeze. There are tons of great tutorials that walk you through creating your first "Hello World" iOS application and explain the process very well. Here are some links to get you started:

- [Build "Hello World" iOS app](https://www.appcoda.com/build-hello-world-app-swift/)
- [First iOS app tutorial](https://www.journaldev.com/10214/ios-hello-world-example-tutorial)

## 4. Where to go from here?

Once you have gotten the hang of Xcode, `UIKit` and how iOS applications are generally structured, you should take the next step to creating slightly more complicated applications that include the use of more `UIKit` components, including `UILabel`, `UIImageView`, and even simple applications that use the MVC design pattern and different view controller types such as `UITableViewController` or `UICollectionViewController`. [Codewithchris has a great tutorial series for beginner iOS development](https://codewithchris.com/how-to-make-an-iphone-app/). There are several articles that are also useful for taking that next step in iOS development, including one from [appcoda](https://www.appcoda.com/learnswift/) and [Apple's own iOS app tutorial](https://developer.apple.com/library/archive/referencelibrary/GettingStarted/DevelopiOSAppsSwift/).

## 5. Useful links and further reading

Here are some links to more advanced topics on iOS development and Swift:
- [iOS animations](https://www.raywenderlich.com/363-ios-animation-tutorial-getting-started)
- [Adding Firebase databse to your iOS app](https://firebase.google.com/docs/ios/setup)
- [Comparisons between different iOS app architectures](https://academy.realm.io/posts/krzysztof-zablocki-mDevCamp-ios-architecture-mvvm-mvc-viper/)
- [Unit testing on iOS](https://www.toptal.com/qa/how-to-write-testable-code-and-why-it-matters)
- [RxSwift and RxCocoa](https://www.raywenderlich.com/900-getting-started-with-rxswift-and-rxcocoa)

Popular frameworks/libraries:
- [Alamofire (HTTP Networking)](https://medium.com/the-traveled-ios-developers-guide/thoughts-on-alamofire-55a52a3d3d57)
- [AFNetworking (HTTP Networking)](https://github.com/AFNetworking/AFNetworking)
- [CocoaLumberjack (Logging)](https://github.com/CocoaLumberjack/CocoaLumberjack)
- [SDWebImage (Image Caching)](https://github.com/rs/SDWebImage)

General iOS development:
- [Ray Wenderlich](https://www.raywenderlich.com/ios)
- [Brian Advent](https://www.youtube.com/channel/UCysEngjfeIYapEER9K8aikw)
- [Let's Build That App](https://www.youtube.com/channel/UCuP2vJ6kRutQBfRmdcI92mA)
- [Appcoda](https://www.appcoda.com/)

</div>

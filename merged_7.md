While  every  platform—Web,  iOS,  Android—is  different,  they  share  some 
similarities. Realizing this will make your job as a React Native developer a little 
easier.
IDEs
First and foremost, when building an app, you need an editor to write and edit code, at the 
very least. For JavaScript, you typically use whatever you are the most familiar with—VSCode, 
WebStorm, Neovim—to name a few. Since these tools are rather unopinionated, it is best 
practice to have your environment configured to work with TypeScript, ESLint, Prettier, and 
React. 
For Android and iOS, it is generally recommended to work with IDEs provided by Google 
and Apple, respectively. While you can write Swift or Kotlin from within VSCode, the 
aforementioned IDEs provide a more predictable and bullet-proof setup overall.
The features that those native IDEs provide out-of-the-box include:
 ■View Hierarchy Debugger—inspects and debugs UI layouts visually, allowing you to 
analyze element positioning, constraints, and rendering issues.
 ■Instrumentation & Performance Profiling—built-in tools for monitoring CPU, 
memory, battery usage, and UI responsiveness, such as Android Studio's Profiler and 
Xcode's Instruments.
 ■Advanced Debugging Tools—LLDB and GDB support in Xcode, Logcat, and 
Debugger in Android Studio, with deeper insights into stack traces, crash reports, and 
memory leaks.
 ■Seamless Gradle (Android) & Build System (Xcode) Integration—manage 
dependencies, build variants, signing, and packaging without extra setup; integrated 
workflows for code signing, app validation, and direct publishing to app stores.
 ■Automated Testing Tools—native unit and UI testing frameworks like XCTest 
(Xcode) and Espresso/JUnit (Android Studio) with built-in test runners.
We will rely on them as we walk through other guides and best practices in this part.
Dependency management
When working with a web or mobile application, we often rely on third-party modules. In 
React Native, a library can be a combination of JavaScript, iOS, and Android dependencies. 
Understanding how dependencies are managed across platforms will help when working 
directly with native modules.
React Native abstracts most of this complexity with autolinking, which automatically detects 
and links React Native-specific modules. However, autolinking only works for libraries designed 
explicitly for React Native. If a dependency has native code for iOS and Android but isn't 
structured as a React Native module, additional steps are required.

> npm install react-native-bottom-tabs
This command fetches the package from the npm registry, adds it to node_modules/,  and  
updates package.json.
Here are some files to look out for:
 ■package.json—defines project dependencies, scripts, and metadata.
 ■node_modules—stores all installed dependencies locally.
 ■package-lock.json—ensures version consistency across environments.
 JavaScript developers often use alternative package managers, such as Yarn, PNPM, 
or Bun.
 iOS
For iOS, React Native uses CocoaPods to manage native dependencies.
Unlike a traditional iOS project, where dependencies are fetched from CocoaPods (Podspec 
repositories) or Swift Package Manager, React Native libraries typically ship their native code 
to the npm registry, and CocoaPods uses local references to locate and load that code. However, 
if you need to add a regular iOS-native dependency that is not a React Native library, you will 
need to fetch it from CocoaPods.
For example, to add SDWebImage to your project, you have to add the following to your Podfile:
pod 'SDWebImage', '~> 5.0'
Let's examine how package management works for JavaScript, iOS, and Android. Understanding 
this process will help you navigate your dependencies and pull in additional native modules 
when needed.
JavaScript
For JavaScript dependencies, React Native primarily relies on the npm registry, a centralized 
repository for JavaScript libraries. This registry contains packages that may include only 
JavaScript code or a combination of JavaScript and native code.
Installing a JavaScript dependency in a React Native project is straightforward using npm:

> cd ios && bundle exec pod install
Note that we're not running CocoaPods directly, but instead using the bundle command. Since 
CocoaPods is a Ruby project, we can leverage Bundler, Ruby's dependency manager, to have 
the most up-to-date CocoaPods version required for a given React Native project, specified in 
a top-level 
Gemfile that comes by default with every new React Native project. 
After installing pods, you can quickly launch Xcode using the xed . command. 
Guide: Understand Platform Differences
If you created a new React Native project with React Native Community CLI, the result may 
look as follows:
require_relative '../node_modules/react-native/scripts/react_
native_pods'
require_relative '../node_modules/@react-native-community/cli-
platform-ios/native_modules'
platform :ios, '12.4'
install! 'cocoapods', :deterministic_uuids => false
target 'HelloWorld' do
  # Here goes your native dependency
  pod 'SDWebImage', '~> 5.0'
  
  # Rest of Podfile
end
This is a typical Podfile from a fresh React Native project with some parts removed for readability;  
you can see the full contents of this Podfile here
The ~> operator in CocoaPods follows a pessimistic version constraint similar 
to SemVer (Semantic Versioning), but with a slight difference:
 ■ ~> 5.0 allows versions >= 5.0 but < 6.0 (patch and minor updates allowed, 
but no major version updates).
 ■~> 5.0.1 allows versions >= 5.0.1 but < 5.1 (only patch updates allowed, 
no minor version updates).
Once you've added your dependencies, go to the ios directory and install pods using the pod 
install command:

Here are some files to look out for:
 ■Podfile—defines dependencies required for the iOS app.
 ■Gemfile—defines CocoaPods version and other required plugins for a given React 
Native app.
 ■Pods—contains all installed dependencies fetched via CocoaPods. This folder 
is generated when you install dependencies and should typically not be committed to 
version control (it is listed in.gitignore in all React Native projects by default).
 ■.xcworkspace—the workspace file generated by CocoaPods, which must be used 
instead of .xcodeproj when opening with Xcode. It ensures that all CocoaPods-
managed dependencies are correctly linked in Xcode.
While Apple's Swift Package Manager (SPM) is an alternative for native iOS 
projects, React Native does not yet support it, making CocoaPods the default 
choice. 
Android
For Android, React Native manages dependencies using Gradle, Android's build automation 
tool. Similar to iOS, React Native-specific modules do not come from external registries 
like Maven Central or JCenter. Instead, they are referenced locally from node_modules/ and 
compiled directly into the project.
For an Android-native dependency that is not a React Native module (e.g., Glide for image 
loading), you must manually add it to the android/app/build.gradle file:
dependencies {
  implementation 'com.github.bumptech.glide:glide:4.12.0'
}
After adding a new dependency in build.gradle, you need to synchronize the Gradle project 
to fetch and apply the changes. 
While Gradle doesn't use ~>, it offers other flexible versioning options:
 ■Exact version—'com.github.bumptech.glide:glide:4.12.0'
 ■Version range (+ operator)—com.github.bumptech.glide:glide:4.+. Uses the 
latest minor or patch update within version 4.x.x, but avoids major updates.
 ■Dynamic versioning—
com.github.bumptech.glide:glide:+. Uses the latest version 
available. Generally not recommended, it may break your code unpredictably.
 ■For production apps, always use exact versions to ensure compatibility, just as 
CocoaPods' ~> prevents breaking changes in iOS projects.

Once completed, you can access a new native dependency in your project.
Additionally, here are some files to look out for:
 ■build.gradle (Project-Level)—located in android/build.gradle, this file configures 
global project settings, including Gradle plugin versions and dependency repositories 
like Maven Central or JitPack.
 ■build.gradle (App-Level)—located in android/app/build.gradle, this file defines 
app-specific dependencies, build configurations, and compile options.
 ■gradle.properties—contains configuration flags and settings to optimize Gradle 
builds (e.g., enabling Hermes or ProGuard).
 ■gradlew & gradlew.bat—the Gradle Wrapper scripts that come with React Native, 
ensuring consistent Gradle versions across all development environments. The .bat file 
is used for Windows systems.
 ■.gradle/ Directory—stores Gradle's cache, compiled scripts, and temporary build 
files. It is generated during the build process and should not be committed to version 
control.
For example, to list available Gradle tasks, you can run ./gradlew tasks.
Building a project
When working with React Native, it's essential to understand how JavaScript, iOS, and Android 
handle code execution and compilation. Unlike native platforms that use compiled languages, 
JavaScript does not have a traditional build system but undergoes a different transformation 
process.
JavaScript
JavaScript is an interpreted language that does not require a separate compilation step like 
Swift or Kotlin. Instead, JavaScript code is executed by an engine at runtime. However, due 
to the multiple ECMAScript versions and evolving JavaScript standards, React Native does 
require a form of  "compilation" known as transpilation to ensure compatibility across different 
environments.
> cd android && ./gradlew clean
React Native projects come with a Gradle Wrapper (gradlew), which ensures all developers 
use the same Gradle version, avoiding compatibility issues. You can run it like this:

In React Native, this process is handled by Metro, the JavaScript bundler that transforms 
modern JavaScript code into a version that the JavaScript engine (e.g., Hermes) can understand. 
Under the hood, Metro relies on Babel, which rewrites JavaScript syntax to match the latest 
supported version of the JavaScript engine used in React Native.
Since React Native depends on a JavaScript engine that evolves over time, the 
exact JavaScript version supported varies based on the Hermes version. React 
Native preset used by Babel ensures that JavaScript is always transpiled to match 
the latest engine capabilities used in React Native.
iOS
Unlike JavaScript, Swift and Objective-C are compiled languages. The iOS build system 
compiles source code into machine code before execution, ensuring optimized performance on 
Apple devices.
React Native for iOS relies on Xcode's build system, which uses Clang (for Objective-C) and  
LLVM (for Swift) to convert human-readable source code into an optimized binary (such as 
.app bundle).
Here are the key steps:
 ■Source Compilation: Swift and Objective-C files are compiled into machine code using 
Clang/LLVM.
 ■Linking: All compiled code, system frameworks, and external libraries (from 
CocoaPods) are linked together.
 ■Signing: Apple requires apps to be signed with valid certificates and provisioning 
profiles before they can be run on a device.
 ■Packaging: The final binary (.ipa file) is generated, containing the app and its 
resources.
Android
Similar to iOS, Android uses a compiled build system, but it relies on Gradle, which automates 
the process of building, testing, and packaging an Android app.
React Native for Android compiles Java/Kotlin source code into Dalvik Executable (DEX) 
format, which is optimized for execution by the Android Runtime (ART).
Here are the key steps:
 ■Source Compilation: Java/Kotlin files are compiled into 
.class bytecode files using 
the Java/Kotlin compilers.
 ■DEX Conversion: The compiled .class files are converted into .dex files, optimized 
for the Android Runtime.

■Resource Processing: XML layouts, images, and other assets are bundled.
 ■Linking: External libraries and dependencies (managed by Gradle) are linked.
 ■Signing: Android requires apps to be signed with a valid keystore file.
 ■Packaging: The final .apk or .aab file is generated for installation on devices.
Running the application on the device
Testing is an essential part of the mobile app development workflow. Unlike web applications, 
which can be tested directly in a browser, mobile apps require simulators (emulators in 
Android) or physical devices for testing. Simulators allow developers to run and debug apps in 
a controlled environment that mimics real-world device behavior.
Android
You can interact with Android emulators using the following methods:
 ■Android Studio AVD Manager: The built-in tool for managing Android emulators. 
You can create, configure, and launch virtual devices here.
 ■Command Line (adb and emulator commands):
 ■emulator -list-avds—lists all available virtual devices.
 ■emulator @Pixel_6_API_34—launches a specific device.
 ■adb devices—lists running emulators and connected physical devices.
 CLI tools such as Expo CLI or React Native Community CLI use these commands 
under the hood to provide a better developer experience.
 iOS
You can interact with iOS simulators via the following:
 ■Xcode: Apple's built-in tool for running iOS apps on a simulated device. You can 
launch it via Xcode > Open Developer Tool > Simulator.
 ■Command Line (xcrun simctl commands):
 ■xcrun simctl list—lists available simulators.
 ■xcrun simctl boot "iPhone 15"—boots a specific simulator.
 ■xcrun simctl shutdown all—shuts down all running simulators.

Launching and managing simulators is something you will do quite often, and 
multiple apps are available to help you work with them outside of Android Studio 
and Xcode. Some that we use daily include: 
 ■MiniSim
 ■Android iOS Emulator for VSCode
 ■Expo Orbit
 ■Shopify Tophat
A screenshot from MiniSim that shows opened simulators (with a tick) and other available simulators too
We hope this chapter, although not strictly correlated with performance optimizations, will 
give you a better overview of the tooling ecosystem React Native engineers operate within. 
The tools listed here are essential for further work on optimizing any React Native app.

GUIDE
HOW TO PROFILE NATIVE 
PARTS OF REACT NATIVE
Profiling is essential not only to JavaScript code, which we covered in the How to Profile JS 
and React Code chapter, but also other languages and platforms, such as iOS or Android. 
Bottlenecks happen on both sides, and when React Native DevTools is not enough, we need to 
turn to the platform's native tooling—Android Studio or Xcode.
In addition to CPU, memory, and network profiling, battery usage is equally important for 
mobile users. It's usually a proxy to the CPU overhead if we remove screen brightness from the 
equation.
iOS
Xcode provides the Debug Navigator view to inspect CPU, memory, disk, and network load at 
a glance. It's located in Xcode's side pane under the "Debug Navigator" icon when your app is 
running and attached to the Xcode debugger:
Xcode's Debug Navigator
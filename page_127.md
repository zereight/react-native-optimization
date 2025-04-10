BEST PRACTICE
SHRINK CODE WITH R8 ANDROID
As mentioned in the How to Analyze App Bundle Size chapter, app size is an important metric 
that you should keep an eye on. In the previous chapters, we discussed how you can optimize 
the size of your JavaScript bundle. However, in some cases, native code can affect the app size 
too. Using tools described in this section can help you shave off additional megabytes from the 
size of your app and make it safer by using obfuscation techniques.
Enable R8
You may have heard of ProGuard. It's a tool in Android Gradle that shrinks, optimizes, and 
obfuscates your app's code to reduce APK size. However, if your project is using React Native 
v0.60 or higher, the Android Gradle Plugin, which is a core dependency for most Android 
projects, doesn't optimize your code with ProGuard anymore. It's using R8. 
R8 is a tool in Android that replaces ProGuard to shrink, optimize, and obfuscate APKs, 
offering improved performance and compatibility with modern features, such as Kotlin, which 
ProGuard didn't support. It's also faster and compatible with ProGuard tooling, such as its 
configuration files, so you may still see some "proguard" references in your Android files, and 
that's fine.
To enable R8 in a React Native project, edit the app/build.gradle file:
def enableProguardInReleaseBuilds = true
Enabling proguard in android/app/build gradle
In the default React Native template, this variable will set the minifyEnabled setting to true 
in the release build variant, which tells Gradle to apply R8 optimizations to your apps when 
bundling for distribution.
In the How to Analyze App Bundle Size chapter, we tested how much enabling R8 with default 
ProGuard rules can shrink the app size. The sample app mentioned there was 9.5 MB without 
Best Practice: Shrink code with R8 android
The Ultimate Guide to React Native Optimization
167
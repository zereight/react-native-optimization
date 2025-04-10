First, let's check where the init function will be called on iOS:
A screenshot from Xcode debugger calling init on the main thread
and on Android:
A screenshot from Android Studio debugger calling init on the JavaScript thread
There is a notable discrepancy between platforms. On Android, the init method was called 
on the JavaScript thread, called mqt_v_js, while iOS used the main thread. The difference 
comes from an assumption made in React Native about modules overriding the init method. 
Whenever we override the init function in an iOS app, React Native assumes that we may 
access UIKit; therefore, it calls the function on the main queue (UIKit is not thread-safe).
React Native source code explaining running init on the main thread
Guide: Understand the Threading Model of Turbo Modules and Fabric
The Ultimate Guide to React Native Optimization
106
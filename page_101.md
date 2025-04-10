A screenshot from Xcode debugger calling invalidate on the Turbo Modules thread
A screenshot from Android Studio debugger calling invalidate on pool-2-thread-1 which is "ReactHost" Thread
We have another small discrepancy between platforms. On iOS, the function is called on the 
Turbo Modules thread, and on Android, the thread doesn't have an explicit name; it's probably 
auto-generated. Going through the source code, you can find that it's a thread spawned by the 
ReactHost class. Now that you better understand the threading model—what is called on which 
thread and why—you'll be able to add advanced, thread-safe, and performant native functionalities to 
your React Native app.
Guide: Understand the Threading Model of Turbo Modules and Fabric
The Ultimate Guide to React Native Optimization
110
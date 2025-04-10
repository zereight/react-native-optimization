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
Guide: How to Profile Native Parts of React Native
The Ultimate Guide to React Native Optimization
76
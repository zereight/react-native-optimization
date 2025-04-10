A simplified threading model of React Native in New Architecture presenting context in which JS is executed
React Native initialization code handles a handful of key mechanisms:
 ■Initialization of React Native internals—cross-platform C++ React renderer, 
JavaScript Interface (JSI), Hermes JS engine, layout engine (Yoga)—on the main 
thread.
 ■Initialization of the JavaScript thread that runs JS and communicates back and forth 
with the main thread through the JSI.
Introduction
In the first part of this guide, we'll focus on the JavaScript part of the React Native ecosystem. 
As you probably already realize, React Native leverages the native platform primitives and 
glues together various technologies: Kotlin or Java for Android, Swift or Objective-C for iOS, 
C++ for React Native's core runtime, and JavaScript executed by the Hermes engine. It's a lot 
to take in, so let's break it down a bit and focus on one platform for simplicity: Android.
When the user opens a native Android app, it typically goes through a similar initialization 
path: the app opens on the main thread, it loads the native code—usually written in Kotlin, 
Java, or C++—into memory, and then executes this code, which usually results in some kind of 
UI being shown to the user. It's not so different for an Android app created with React Native. 
After all, it's a native app but with a twist: part of the native code being executed comes from 
React Native.
JavaScript initialization path
The Ultimate Guide to React Native Optimization
11
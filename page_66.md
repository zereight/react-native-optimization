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
Guide: Understand Platform Differences
The Ultimate Guide to React Native Optimization
73
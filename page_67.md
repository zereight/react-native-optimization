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
Guide: Understand Platform Differences
The Ultimate Guide to React Native Optimization
74
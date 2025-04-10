optimizations. After enabling R8, we got 6.3 MB, a 33% difference. Larger projects should 
expect slightly smaller improvements, but it is worth doing so.
ProGuard rules and configuration for R8
As we already mentioned, R8 is configured with "ProGuard rules". In a dedicated file, you can 
control which classes, methods, and fields should be kept (preserved) or discarded during the 
build optimization process. Additionally, the rules let you configure code obfuscation patterns 
(like class/method renaming) to make reverse engineering more difficult. In a React Native app, 
these rules are defined in app/android/proguard-rules.pro. This file is empty by default. 
In most cases, you don't need to define any rules manually. However, if you run into issues 
with your app not working correctly after dead code elimination, you may need to add classes 
to exclude. This may be necessary for certain dependencies making assumptions about class 
names. For example, if you are running into issues using Firebase, you can tell R8 to keep those 
classes (prevent removal):
# Firebase
-keep class io.invertase.firebase.** { *; }
-dontwarn io.invertase.firebase.**
proguard-rules.pro file
Obfuscation is enabled by default when specifying minifyEnabled. If, for some reason, you 
want to disable it, use -dontobfuscate setting in your proguard-rules.pro file:
-dontobfuscate
proguard-rules.pro file
You can read more about defining ProGuard rules in the official documentation.
Shrinking resources
Once you have R8 enabled in your release build variants, you can unlock further optimizations 
thanks to Gradle's resource minification. This can reduce the app bundle even further. When 
enabled, it performs several optimizations:
1.  Merges duplicate resources.
2.  Optimizes PNG files by reducing their color depth and applying lossless compression.
3.  Processes vector drawables.
Best Practice: Shrink code with R8 android
The Ultimate Guide to React Native Optimization
168
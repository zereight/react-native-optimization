It can be enabled inside of app/build.gradle file using shrinkResources setting:
android {
    buildTypes {
        release {
            // Enables code shrinking, obfuscation, and 
optimization for only
            // your project's release build type. Make sure to 
use a build
            // variant with `debuggable false`.
            minifyEnabled true 
            
            // Enables resource shrinking, which is performed by 
the
            // Android Gradle plugin.
            // Make sure `minifyEnabled` is set to `true`
            shrinkResources true
        }
    }
}
app/build.gradle file
Now you know how to make your app smaller by enabling dead code elimination. Remember 
you can use the Ruler tool to verify the difference between your app's downloaded and installed 
size. Make sure to give your app a good round of testing before shipping it to the store with 
this technique enabled.
Best Practice: Shrink code with R8 android
The Ultimate Guide to React Native Optimization
169
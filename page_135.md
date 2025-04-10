BEST PRACTICE
DISABLE JS BUNDLE 
COMPRESSION
We already learned that APK and Android App Bundles are compressed archives. The Android 
build system applies compression to files during packaging primarily to reduce the overall size 
of the APK or AAB, which can be beneficial for reducing download sizes and storage usage on 
devices. It does so without necessarily distinguishing between file formats, beyond generally 
skipping already efficiently compressed formats like image files (e.g., .png, .jpg).
In an Android React Native app using Hermes, the final JS bundle is packaged into the index.
android.bundle file. Although binary, it might be treated like other resources packaged in 
the app because the build system applies compression to most resource files by default unless 
explicitly told not to.
What's intriguing is that—as a compressed file—it will be skipped during the memory mapping 
(mmap) procedure, as investigated in this Pull Request to the React Native core. This prevents 
one of the most important Hermes optimizations from working properly, negatively affecting 
the TTI metric.
As of writing this note (February 17, 2025), the React Native Core Team is 
including this behavior by default. It's most likely going to be available in React 
Native 0.79.
To opt out of this behavior, regardless of the React Native version used, and fully leverage 
Hermes potential, you can manually tell the Android build system to disable compression for 
files with 
bundle extension by setting androidResources in your app/build.gradle file:
android {
 +    androidResources {
Best Practice: Disable JS Bundle Compression
The Ultimate Guide to React Native Optimization
175
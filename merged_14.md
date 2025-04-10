> ./gradlew bundleRelease
Running this command will produce an Android App Bundle that the Play Store can use to 
produce application binaries on-demand, optimized for the user's device architecture. If you 
want to learn more about this and other formats, head back to the Introduction.
Once  done,  head  over  to android/app/build/outputs/bundle/release/app-release 
folder. In order to inspect the contents of the bundle, we will unpack it by changing its extension 
to .zip first (after all, AAB is just a ZIP archive).
A look inside Android App Bundle after unpacking its contents
After heading to the resources catalog, you will see multiple folders with names corresponding 
to Android screen densities, such as 
drawable-xhdpi-v4, and drawable-xxhdpi-v4. Looking 
at the images inside, you will notice your suffixes, such as @2x are gone. Instead, images were 
located in each folder based on the best density available.
When a user downloads the application from Google Play, the on-demand application for their 
respective device will contain assets for that density only, without everything else.
Native assets folders
As long as your images have size suffixes, Android will apply this optimization automatically. 
Let's use this as an opportunity to look under the hood and better understand what is going on. 
To do so, we will first create an AAB file by running the following command:

Xcode Asset Catalog
We can achieve similar results on iOS. However, unlike on Android, it is not done automatically 
by React Native and Metro while creating the bundle. In order to benefit from similar 
optimization, we must explicitly create an Xcode Asset Catalog (.xcassets) and configure it as 
part of our compilation pipeline.
To get started, let's create the RNAssets.xcassets file and place it inside the ios folder.
This folder has to be named RNAssets.xcassets due to the hard-coded path 
inside React Native bundle scripts. Hopefully, this constraint will be dropped in 
the future.  
After creating the folder, navigate to Build Phases from within your Projects Settings and 
modify the Bundle React Native code and images build phase with EXTRA_PACKAGER_
ARGS variable above line 8:
export EXTRA_PACKAGER_ARGS="--asset-catalog-dest ./"
Bundling code and images happens as one of build phases

As a result, when that build phase gets executed, Metro will place assets inside RNAssets.
xcassets catalog. After running the build for the first time, your asset folder should look 
like this:
A look into Xcode Asset Catalog with sample images
You can also trigger the bundle command manually if you run into issues or want to test if 
everything works (notice that --asset-catalog-dest now points to ios because we run this 
script from the root of the React Native project):
> npx react-native bundle \
  --entry-file index.js \ 
  --bundle-output output.js \ 
  --platform ios \
  --dev false \
  --minify true \ 
  --asset-catalog-dest ios \
  --assets-dest <your-assets-folder>
Now, after uploading your app to the App Store, the app thinning mechanism will kick in and 
choose proper image sizes based on the user's device!
On iOS, "App Thinning" optimizes app delivery by creating device-specific 
versions, including only required assets. Android achieves similar results through 
"Android App Bundles" (AAB), which generate optimized APKs for each device 
configuration, breaking apps into modular components that can be downloaded 
on demand. 
Testing it out
Let's examine the real difference on iOS, before and after applying this optimization. Without 
it, your bundle will include all three images, which is a waste because your device will only use 
one size.

A screenshot from Emerge Tools before optimization of assets catalog on iOS
After applying the abovementioned technique and using the assets catalog, you will see that 
only the relevant image (assets_image_image@3x.jpg) is present in the bundle.
A screenshot from Emerge Tools after enabling app thinning
It's a great optimization technique that ensures your app includes only necessary images for 
a given platform. Overall, it has a positive impact on the app size metric.
At the time of writing this chapter (January 2025), Assets Catalog is not enabled 
by default; however, there are ongoing efforts to make it the default choice.

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

+        noCompress += ["bundle"]
 +    }
  }
app/build.gradle file with compression disabled for .bundle files
As a result, the index.android.bundle file won't be compressed, effectively increasing the 
overall app bundle size. This size increase will affect the install size of your app but not the 
download size. The most important bit is that your app should load significantly faster now 
due to Hermes being able to leverage memory mapping and load the JS bundle faster. In our 
initial testing, for an APK of 75.9 MB install size, it was a 6.1 MB size increase (+8%), while the 
TTI dropped by 450 ms (-16%).
It's possible, but not very likely, that your app won't grow in size for various reasons. Measure 
the outcomes and observe your users' behavior to ensure this change is a net positive rather 
than a net negative. Most likely, though, the benefits will outweigh the cost.

ACKNOWLEDGEMENTS
The Ultimate Guide to React Native Optimization 
was brought to you by the Callstack team
Mike Grabowski, CTO and Founder at Callstack
Passionated about cross-platform technologies. When not working, find him on a race track.
 @grabbou     @grabbou
Oskar Kwaśniewski, Senior Software Engineer
Passionate about bridging the gap between Native and JavaScript development. React Native 
Core Contributor, creator of React Native visionOS and React Native Bottom Tabs. In free 
time, he enjoys doing outdoor sports & reading. 
 @o_kwasniewski     @okwasniewski
Robert Pasiński, Senior Software Engineer
Passionate about all things development, low-level coding enthusiast.
 @rob_pasinski    @robik
Michał Pierzchała, Principal Engineer
Passionate about building mobile and web tooling in the Open Source. Core React Native 
Community contributor.
 @thymikee    @thymikee
Jakub Romańczyk, Senior Software Engineer
JS Infra nerd. Passionate about React Native, OSS & JS tooling. Souls-like games enthusiast.
 @_jbroma    @jbroma

Szymon Rybczak, Software Engineer
18-year-old, passionate about open-source technologies around universal apps. 
 @szymonrybczak    @szymonrybczak
Piotr Tomczewski, Expert Software Engineer
Father of two cats, a foodie, will code for LEGO.
 @Piotrski    @piotrski
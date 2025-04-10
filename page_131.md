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
Best Practice: Use Native Assets Folder
The Ultimate Guide to React Native Optimization
171
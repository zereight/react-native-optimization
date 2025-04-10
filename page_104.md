Debugging the view hierarchy
While working on view flattening, you might find view hierarchy debugging useful. Thankfully, 
you can inspect it on both iOS and Android platforms with Xcode and Android Studio, 
respectively.
Xcode
When you run your app through Xcode, you'll notice a debugging toolbar which, on top of 
standard breakpoint debugging, offers Hierarchy inspector. You can launch it by clicking the 
"Debug View Hierarchy" button:
Debug View Hierarchy button in Xcode
This stops your app, as it was stopped on a breakpoint, but now you can inspect the native 
view hierarchy:
View hierarchy of a sample React Native app; it really is a native app!
Seeing a visual representation of the view hierarchy can be helpful when solving layout-
related issues. If you look at the left sidebar, it lists the native views corresponding to 
JavaScript components, so a JavaScript <View/>  is  RCTViewComponentView, the same 
native building block as inside of fully native apps.
Best Practice: Use View Flattening
The Ultimate Guide to React Native Optimization
115
Android Studio
Same as with Xcode, we have a way to inspect the view hierarchy of our app; in Android, it's 
called "Layout Inspector". To launch it, navigate to the top menu bar and open View > Tool 
Windows > Layout Inspector.
Layout Inspector in Android Studio
Here, you can see a similar UI to what we have in Xcode. In the sidebar, there is a list of elements 
currently visible on the screen. You can notice that on this platform, <View/> from JavaScript 
corresponds to ReactViewGroup that holds ReactTextViews inside.
Being aware of how view flattening works is a useful skill. Most of the time, you shouldn't 
think about it as the idea behind it was to be fully transparent. Sometimes, however, you might 
run into issues where this optimization was misapplied, and the layout debugger should help 
you understand what's happening. And, if necessary, skip flattening of problematic views.
Best Practice: Use View Flattening
The Ultimate Guide to React Native Optimization
116
Available tools within Xcode's Instruments
It will open a new window. To start profiling your app, click on the dropdown menu next 
to the target device (here, iPhone 16 Pro), select the app (SampleApp), and hit the recording 
button (red circle):
Select profiling target
The profiler will restart your app and start collecting CPU samples for profiling from the start. 
Now, you can use your app in a way that reproduces a performance regression you or your 
users found, and you want to investigate. Once you have enough data, stop the recording in the 
Instruments window.
In our case, we're rendering an application that presents 5,000 views in a ScrollView and is 
re-rendered with every state update when a button is pressed. On top of that, we'll toggle an 
image that will push the list down, causing a re-layout. 
Scroll down to find the Time Profiler tool, which includes CPU Profiler.
Guide: How to Profile Native Parts of React Native
The Ultimate Guide to React Native Optimization
78
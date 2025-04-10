Output of the CPU Profiler on a SampleApp, showing the breakdown of main thread, JS thread, and detected hangs
There's a lot to break down here. We've collected 27 seconds of data, but we're only interested 
in a small portion of that time. Using the Cmd + shortcut, we can zoom in on the timeline to 
our points of interest and horizontally scroll with a mouse or a touchpad. 
In our case, we're focusing on ~1.3s of our total time when we press a button and wait for the 
app to update. You can notice a brief spike on the UI thread (labeled as SampleApp), followed 
by some intensive CPU work on the JavaScript thread and a lot of work on the UI thread. The 
profile is displayed as a flame graph on the bottom, showing React Native's internal call sites 
(we filtered out the system libraries).
Notice how during the high CPU workload on the UI thread, there's a "Microhang", while 
a high JS thread load doesn't. This indicates that the UI thread is doing a lot of work, and it 
won't allow for immediate interactions with our content. It could be worse, though—if the 
thread was fully blocked, it would be displayed as a "Hang" and should be a high priority for 
you to investigate. The Hang tool accompanies the Time Profiler and can serve as a quick way 
to notice problematic parts of our app flow.
You can effectively block the JS thread, and users will still be able to interact with 
native UI elements of your app, e.g., press a button. It just won't communicate 
back to the JS thread immediately. It's a nice design implication of React Native 
that we often take for granted and is worth appreciating. 
The profile of such interaction looks like this in the Instruments CPU Profiler:
Guide: How to Profile Native Parts of React Native
The Ultimate Guide to React Native Optimization
79
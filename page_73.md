Toggle between Call Tree and 
Flame Graph view
Call Tree view of the samples
You can toggle between a Call Tree and Flame Graph view, which you already may know from 
React Native DevTools. In our initial screenshot, we pinned the JavaScript thread to see how 
it correlates with the UI thread and hangs that were detected.
Press the plus sign to pin the thread to have easier way comparing it with others on the 
timeline
The CPU Profiler tool allows for quite a few customizations to help you pinpoint the source 
of the problem. For example, you can filter the Call Tree so it hides system libraries or invert 
the call tree altogether to access a bottom-up view you may know from other profilers that you 
can sort by weight expressed in "counts" (where, e.g., Mc is Mega counts, meaning millions of 
samples collected). It essentially allows you to see the functions where most of the time is spent, 
regardless of which higher-level function called them. You can also inspect the calls by thread 
if that helps.
Helpful Call Tree filters you 
can toggle to quickly find most 
frequent function calls
Thanks to the CPU profiler, we can conclude that storing 5k elements in a ScrollView isn't 
optimal for our app's performance and we need to do something about it.
Android
For the Android platform, we can use Android Studio and built-in tools to profile our app's 
CPU, memory, network, and battery usage.
Android Profiler
Android Studio is the IDE developed by JetBrains. It is officially supported by Google and an 
official Android IDE, which can be used to develop any Android app. It is very powerful and 
contains many functionalities in one place, including Android Profiler.
Guide: How to Profile Native Parts of React Native
The Ultimate Guide to React Native Optimization
80
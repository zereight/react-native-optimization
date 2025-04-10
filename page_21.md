> flashlight measure
This allows you to monitor memory usage and when the FPS slows down. There are actually 
two FPS monitors—one for the UI (or Main) thread and another for the JS thread. This allows 
you to quickly realize whether a particular interaction slowdown originates from JavaScript or 
native.
Always disable development mode when measuring performance. 
On Android, it's accessible from Dev Menu in Settings > JS Dev Mode.
On iOS, you'll need to run Metro in dev mode.
As we learned in the How to Profile JS and React Code chapter, when the FPS drops during 
animation or some operation, you can use a profiler to narrow down the cause.
Flashlight
FPS in the Dev Menu is easy to see but hard to track and measure. It's quite convenient to get 
a view of, e.g., the average FPS for a particular interaction or screen load. This is where tools 
like Flashlight shine. It works only on Android, though.
To get started, make sure you have Flashlight installed and available in your terminal. Open up 
the Android application you want to measure on a given Android emulator, and then run: 
You will see the diagram with various performance metrics appear in real time as you interact 
with the application. 
Getting an average FPS is only one of the features available in a lighthouse-like 
report produced by the tool. The report also includes the app's overall performance 
score based on the metrics collected during the measurement, such as the average 
FPS number, average CPU, and RAM usage.
All collected measurements are saved in a JSON file that you can store and compare between 
different versions of your code. It's a good solution for automated local performance monitoring 
and to prove that the FPS has improved reliably.
Guide: How to Measure JS FPS
The Ultimate Guide to React Native Optimization
23
Now, you should be able to connect to a running profileable app on your device. We'll use 
"Find CPU Hotspots" task and hit that "Start profiler task" button to start collecting samples 
from our app.
It's worth noting that the Android ecosystem of devices is way more fragmented 
than iOS. Mobile phones, tablets, TVs, etc., range from low-end through mid-end 
and high-end. Pick the lowest-end device available or emulator for profiling. Use 
data from real-time user monitoring if possible.
We'll perform the same scenario as with the iOS app: press our button a few times to cause 
a re-render of a giant list we have. When you stop profiling, the UI will change to a flame graph 
showing a breakdown of the work done on the CPU and various threads. 
Let's zoom in on that JS thread right after we press the button; we can see an indication of 
higher CPU usage:
A zoomed-in view of the JS thread showing a lot of work performed by Hermes re-rendering the views
We can observe a lot of tiny 1ms spikes of activity coming from the Hermes JS engine going 
through all the 5k views we'll need to eventually draw on the screen. All that work sums up to 
over 240 ms, which is way more than a 16.6 ms budget to get at least 60 FPS. Notice how around 
a third of the total time is spent in the "commit" phase of the React reconciliation algorithm, 
which lays out new views with Yoga, to be then mounted and drawn onto the screen.
And if we scroll up a little on the flame graph to see the main thread, we can notice an event 
handler for pressing the button down, followed up by another one when releasing the finger.
Event handler on the main thread that starts the expensive work on JS thread later on
The touch release timing almost perfectly matches the start of the increased work on the JS 
thread, which is achieved through the sync JSI calls internally by React Native. In the legacy 
architecture, that information would be serialized and broadcasted by the bridge, adding extra 
overhead to this interaction. 
As with any other profiling tool, we can also access a top-down or bottom-up analysis of the 
call tree, where we can additionally filter by libraries and calls that interest us.
A bottom up view of call sites filtered by the "hermes" keyword
It's worth remembering that profiling tools such as Android Studio or Chrome JS Profiler 
allow you to export the trace and load it in a third-party tool. 
Guide: How to Profile Native Parts of React Native
The Ultimate Guide to React Native Optimization
82
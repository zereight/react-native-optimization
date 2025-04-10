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

To open the Profiler, choose View > Tool Windows > Profiler from the Android Studio 
menu bar:
View > Tool Windows > Profiler menu
Or click "Profiler: Run 'app' as profileable" in the Android Studio's toolbar. If that doesn't 
work, you'll either need to run a debuggable build or create and run a profileable version of 
your app yourself using official Android Developer guidelines.
Android Studio Profiler with a handful of tasks like Find CPU Hotspots or Track Memory Consumption

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

Perfetto
One such tool that might come in handy is Perfetto, which allows you to do system profiling, 
app tracing, and trace analysis by loading previously exported traces. It supplies Android Studio 
Trace Viewer with additional features. You can upload the trace through the online viewer 
available at ui.perfetto.dev and further inspect it from a different angle.
Android Studio-generated memory trace displayed in the Perfetto web app
Knowing how to use a profiler is one of the most crucial skills you can learn to help yourself fix 
any kind of performance issue at any codebase. It's especially helpful for the code you haven't 
even written. But with the right tools, you can understand its performance characteristics and 
apply optimizations where needed.

GUIDE
HOW TO MEASURE TTI
The app's Time to Interactive (TTI) metric is one of the two most important metrics every 
app should track. It tells us how much time elapses from touching the app icon to displaying 
meaningful content that users can interact with through touch, voice, or other input methods. 
Users expect this process to be as fast as possible, ideally instant. 
Various reports of mixed quality indicate that users typically expect an app to load in under 
2 to 4 seconds; otherwise, they will drop for alternatives (if available). Our experience tells 
us, however, that you should trust your data and take external reports with a healthy dose of 
skepticism. The same experience also tells us that the TTI should be as low as possible. 
But where to draw the line between the optimal effort and outcome? Is it worth spending 
months of the engineering team's capacity to get from 2.9s to 2.3s on a reference Android 
device? Your particular user base is unique, and only observing user behavior can give you an 
answer. In this chapter, we'll focus on the techniques to get you to the lowest TTI possible.
Measuring TTI reliably
Depending on the underlying OS and its capabilities, our app can perform a cold start, warm 
or hot start, or it can be prewarmed. This means the same news app you open every day around 

8 AM, which normally takes 10s to run on iOS when "cold", may start booting in just under 6 
seconds after a few days. That's quite a difference.
A diagram showing how Android putting an app into the background can influence different stages of React Native app startup pipeline
If we naively measured the TTI from every app start to its full interactivity, we would get vastly 
different measurements that would give us no insights into whether we regressed or improved 
this metric. 
That's why it only makes sense to measure the app in the cold boot state and exclude 
measurements from all other boot states: prewarm, warm, or hot. Thankfully, iOS and Android 
creators provide their developers with means to deal with this problem.
Setting up performance markers
Each phase of the React Native app startup pipeline can be instrumented by a performance 
marker—a piece of code that collects information about the time certain events happen. In 
React Native applications, we want to focus on at least the following stages of this pipeline:
 ■Native Process Init—described by nativeAppStart and nativeAppEnd markers.
 ■Native App Init—described by appCreationStart and appCreationEnd markers.
A diagram showing how iOS prewarming can influence different stages of React Native app startup pipeline
In another scenario, if we open the app in the morning and come back to it in the evening, it 
may remain in the background. Now that we're accessing it, it's only going to render the React 
part, significantly speeding up the whole process compared to a cold start.

■JS Bundle Load—described by runJSBundleStart and runJSBundleEnd markers.
 ■React Native Root View Render—described by contentAppeared marker.
 ■React App Render—described by screenInteractive marker.
To record these markers across iOS, Android, and React code, you will need a native module 
or a third-party library. The one that we're mostly happy with is react-native-performance. It 
exposes a RNPerformance class to iOS and Android, allowing you to define custom markers:
import ReactNativePerformance
RNPerformance.sharedInstance().mark("myCustomMark")
Custom marker on iOS
import com.oblador.performance.RNPerformance;
RNPerformance.getInstance().mark("myCustomMark");
Custom marker on Android
They are later available to React code using performance API, just like on the web:
import performance from 'react-native-performance';
performance.measure('myCustomMark');
performance.getEntriesByName('myCustomMark');
// returns: [{ name: "myCustomMark", entryType: "myCustomMark", 
startTime: 98, duration: 2137 }]
Instead of defining custom markers, you can use built-in ones provided by the react-
native-performance library, such as nativeLaunchStart  or  runJsBundleEnd. 
It's worth noting that the nativeLaunchStart is measured pre-main, so during 
prewarming phase, and the rest of the markers after prewarm. You may need to 
filter it out or create a custom one in main().
Native Process Init
First, determine whether it's a cold start. On iOS, you can use ProcessInfo  to  get  this  
information:
let isColdStart = ProcessInfo.processInfo.environment["ActivePrewarm"] 
== "1"
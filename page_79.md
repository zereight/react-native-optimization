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
Guide: How to Measure TTI
The Ultimate Guide to React Native Optimization
87
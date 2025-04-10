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
Guide: How to Measure TTI
The Ultimate Guide to React Native Optimization
86
The CPU monitor measures the amount of work done over time. The percentage value shows 
usage relative to a single core, so it can exceed 100%, especially in React Native apps. The Memory 
Monitor observes the application's memory usage. All iOS devices use SSD for permanent 
storage—accessing this data is slower compared to RAM. Disk Monitor understands your 
app's disk-reading and writing performance. Network Monitor analyzes your iOS app's TCP/
IP and UDP/IP connections.
You can tap on each of them to find more information. It also provides an extra monitor that 
isn't shown by default but can help you inspect your UI—it's the View Hierarchy, which we 
cover in the Use View Flattening chapter.
Instruments
Xcode comes with a pre-packaged debugging and profiling tool called Instruments. It is a suite 
of inspection tools, each serving a different purpose. You choose from a list of templates, and 
you choose any of them depending on your goal: improving performance, battery life, or fixing 
a memory problem. For CPU profiling, we are going to use the Time Profiler. Let's dive into it. 
With Xcode open, go to Instruments from Xcode > Open Developer Tool > Instruments menu.
Xcode > Open Developer Tool > Instruments menu
Guide: How to Profile Native Parts of React Native
The Ultimate Guide to React Native Optimization
77
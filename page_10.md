■Initialization of the Native modules thread that runs lazily loaded Turbo Modules.
Notice how rendering logic (Kotlin, C++) and the so-called "business logic" are, by design, run 
on separate threads—the main thread vs. the JS thread. Since this part is about JavaScript and 
React, we'll focus on the JavaScript side that enables React to run on a dedicated JS thread and 
how this model affects the performance of your React Native apps. 
According to an internal survey of 100 React Native developers at Callstack, 80% of the 
performance challenges they face in mobile, TV, and desktop React Native apps originate from 
the JavaScript side. This sample may not be representative of the real world due to its small 
size. However, seeing what the React Native Community is struggling with, it's pretty safe to 
assume that most of the performance issues are related to JavaScript. As such, it's wise to focus 
on these issues before moving to more advanced native optimizations described in Part 2: 
Native.
React re-rendering model
React takes care of rendering and updating the application UI based on its state for you, regardless 
of the environment (or platform): web, iOS, Android, or anything else. The react library 
itself is tiny and consists mostly of public API definitions, some cross-platform functionality, 
and a reconciliation algorithm, which is responsible for efficiently updating the UI to reflect 
component state changes, effectively describing "what to do".
What makes this model powerful, though, is splitting the "what" from the "how" and deferring 
the latter to specialized renderers that manage how these updates are applied to different 
environments: react-dom for the web, react-native for iOS and Android, or react-native-
windows for Windows, to name a few. This model allows you to define components of your 
app universally for all supported platforms simultaneously, and compose the final interface 
out of these smaller building blocks. With such an approach, you don't control the application 
rendering lifecycle; React and its renderer do the job for you.
Visual description of React reconciliation algorithm
The Ultimate Guide to React Native Optimization
12
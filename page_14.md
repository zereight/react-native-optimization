Now, open the React Native DevTools. Go to the Profiler tab on top. Press the "gear" button 
to configure it to "Highlight updates when components render"
General profiler settings accessed from "View Settings" icon in the Profiler tab of React Native DevTools
and "Record why each component rendered while profiling".
Profiler settings accessed from "View Settings" icon in the Profiler tab of React Native DevTool
Finally, press the "Start profiling" or "Reload and start profiling" button on the top left of the 
screen to start profiling. It doesn't matter in our use case since we're inspecting reactions to 
user input. However, when profiling an app's startup, "Reload and start profiling" is a lifesaver.
"Start profiling" button.
In our sample app, click both buttons a few times and observe real-time feedback from the 
DevTools highlighting components that are being re-rendered. The screenshots reflect a profile 
produced by first pressing "Press one" twice and then "Press two" twice, producing four 
subsequent "commits" by the React renderer.
You can learn more about React's render and commit process in the official docs.
Guide: How to Profile JS and React Code
The Ultimate Guide to React Native Optimization
16
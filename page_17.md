focus on profiling the CPU, examining what's on the call stack, and determining how you can 
detect long-running operations. If you want to inspect memory-related issues, you can check 
the How to Hunt JS Memory Leaks chapter.
After launching React Native DevTools, you can go to the "JavaScript Profiler" Tab and start 
recording the JavaScript CPU Profile.
If you don't see the "JavaScript Profiler" tab, navigate to settings by clicking on 
the gear icon at the top right, and from the experiments section, enable JavaScript 
Profiler.
At the top left, you can select the preferred view. You can choose from Chart, Heavy (Bottom-
Up), or Tree. Depending on what you want to achieve, experiment with each view.
Let's experiment with this long-running function:
  const longRunningFunction = () => {
    let i = 0;
    while (i < 1000000000) {
      i++;
    }
  };
Start recording the profile and stop once it's finished. Once the profile is created, we can see that 
the longRunningFunction took over 12 seconds to run, combined with the whole call stack, 
which led to calling this function. This effectively makes our JS part of the app unresponsive 
for those 12 seconds, but the native UI part should still be responsive due to React Native's 
threading model. 
In mobile apps, we want our functions never to block the JS thread for more than 
16 ms to achieve 60 FPS and, ideally, 8 ms for 120 FPS, which is getting more 
popular. Read more about FPS in the How to Measure JS FPS chapter.
Profiler output showing a flame chart with long-running longRunningFunction taking over 12 seconds to run
If the Chart (or flame graph, as React DevTools calls it) information is hard to read, you can try 
other views, such as Heavy (Bottom Up), where you'll be able to sort by the longest or shortest 
running function calls.
Guide: How to Profile JS and React Code
The Ultimate Guide to React Native Optimization
19
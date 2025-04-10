Here is what the profiler looks like after capturing a snapshot:
Output from the Allocation instrumentation tool showing a potential memory leak with blue bars 
There are a few things to keep in mind about the Allocation instrumentation tool: 
 ■Blue bars at the timeline represent allocations.
 ■If the blue bar turns grey, the object is deallocated.
 ■Below, we have a list of list of constructors that were called. Right next to it, there are 
two metrics:
 ■Shallow size—the size of memory that is held by the object.
 ■Retained size—the memory size that will be freed after the object is deleted.
As you can see, over 30 seconds and multiple calls to the createManyLeaks function, GC 
cleared only some objects from the memory (shown on the graph as grey), but the blue parts 
are still alive. This graph screams that multiple leaks are happening.
Let's focus on the first spike and investigate what's causing this. 
Focused view of allocation spike
Now that the graph is focused on the first spike, we can see which constructors are being called. 
Let's expand the Function constructor and see what we have there.
Guide: How to Hunt JS Memory Leaks
The Ultimate Guide to React Native Optimization
28
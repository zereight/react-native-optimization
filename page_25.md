newDataLeak.forEach((data) => data.id); // Closure captures 
newDataLeak
  };
};
// Create many closures that each capture their own large data
const createManyLeaks = () => {
  for (let i = 0; i < 10; i++) {
    const leakyClosure = createClosure();
    leakyClosures.push(leakyClosure);  // Store reference to 
prevent GC
    // Trigger all closures to keep data "active"
    leakyClosures.forEach(closure => closure());
  }
};
Code showing the third issue from the previous section (closures retaining reference to large data) 
Once you launch DevTools, look for the "Memory" tab and select "Allocation instrumentation 
on timeline". Then, you can click "Start" at the bottom.
Memory panel in React Native DevTools
You have a leak when you finish profiling, and an object is still allocated in memory! But as 
mentioned before, GC runs periodically, so your objects might not get deallocated instantly. 
You might need to allocate something else to trigger the deallocation of existing objects.
Guide: How to Hunt JS Memory Leaks
The Ultimate Guide to React Native Optimization
27
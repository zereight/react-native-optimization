The discrepancy between shallow and retained size for JSFunction
As you can see, there is a call to the JSFunction() constructor whose shallow size is 40 bytes, 
but the retained size is 2 772 088 bytes(!). It's the leak that we were looking for! This closure 
is tricking GC into thinking that the objects are still relevant when they are not. DevTools even 
show us the exact line where this constructor was called. 
After finding the leak, you should fix it and re-test. Let's alter the createManyLeaks function 
and comment-out pushing closures to an array to prevent retaining them.
const createManyLeaks = () => {
  for (let i = 0; i < 5; i++) {
    const leakyClosure = createLeak();
    leakyClosure(); // just call it
    // leakyClosures.push(leakyClosure); 
    // leakyClosures.forEach(closure => closure());
  }
}
As you can see, all the bars are now grey (except the last one, which wasn't yet deallocated), 
which means there are no leaks!
Memory allocation profile with no leaks 
Now you know how to track down and fix memory leaks! In the next chapter, we will focus on 
Controlled Components.
Guide: How to Hunt JS Memory Leaks
The Ultimate Guide to React Native Optimization
29
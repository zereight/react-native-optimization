Leverage background threads
In a browser context, we typically deal with one thread. We can, of course, offload some JavaScript 
work to a web worker, but it's a bit different. In React Native, we can leverage the power of 
devices in the user's hands. By default, synchronous Turbo Module methods get called on the 
JavaScript thread, as you can see below on iOS:
A screenshot from Xcode stopped on a breakpoint
Best Practice: Make Your Native Modules Faster
The Ultimate Guide to React Native Optimization
126
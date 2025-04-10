Asynchronous function calls
Now, let's see how asynchronous functions are called. Similarly to web context, when we await 
a function—let's say fetch—the call gets delegated to the browser's Web APIs (the native 
code layer), which handles this request. The request is handled outside of the main JavaScript 
execution thread, which in React Native is the native modules thread. 
Let's see it in action for an async multiplyOnBackgroundThread function:
A screenshot from Android Studio debugger calling multiplyOnBackgroundThread on the Turbo Modules thread
As you can see, React Native offloads the JavaScript thread for us and calls the function on 
the Turbo Modules thread. However, this thread might be busy as it is shared across all native 
modules. In this case, you can always create a new thread to mitigate the issue.
The last part is to check how modules are invalidated. In order to support invalidation on iOS, 
our module has to conform to RCTInvalidating protocol.
Invalidation happens when the React Native instance is torn down, for example, 
when we reload the Metro server.
A screenshot from Xcode debugger calling multiplyOnBackgroundThread on the Turbo Modules thread
Guide: Understand the Threading Model of Turbo Modules and Fabric
The Ultimate Guide to React Native Optimization
109
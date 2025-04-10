The same happens on Android:
A screenshot from Android Studio stopped on a breakpoint
The deep dive into threading in a React Native app is covered in the Understand 
the Threading Model of Turbo Modules and Fabric chapter.
Now, we want to use that multithreading and schedule some work to be done on a background 
thread. Let's introduce a new asynchronous method multiplyOnBackgroundThread.  To keep 
this guide concise, we will skip the details on adding a new method and focus on offloading 
code to a background thread.
Let's start with iOS. To run our code on a background thread, we can use a higher-level concept 
of queues, available in the form of DispatchQueue API, which manages the scheduling and 
execution of tasks, ensuring they're not run on the main thread. The code available in the 
DispatchQueue.global() block will be executed once the global concurrent queue for 
background work is available. Inside the block, we can call the resolve() method to notify 
our JS function about the result.
 @objc func multiplyOnBackgroundThread(
    _ a: Double,
    b: Double,
    resolve: @escaping RCTPromiseResolveBlock,
    reject: RCTPromiseRejectBlock
  ) {
    DispatchQueue.global().async {
      resolve(a * b)
    }
  }
Example use of DispatchQueue.global() in Swift
Let's see how to achieve the same thing on Android. Much like with Swift, we won't access 
a background thread directly. Instead, we'll use coroutines. They are a part of Kotlin's 
concurrency framework and provide a way to write asynchronous code that is both readable and 
maintainable. It's worth noting that coroutines are not bound to a physical thread—thousands 
of coroutines can run on a small number of threads.
Best Practice: Make Your Native Modules Faster
The Ultimate Guide to React Native Optimization
126
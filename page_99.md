As expected, we are now on a different thread. It's mqt_v_native—a separate thread for Turbo 
Modules. It's worth keeping this in mind if you plan to access a JavaScript instance here. In 
that case, you would need to use JavaScript CallInvoker to schedule a function call on the JS 
thread.
Synchronous function calls
With initialization out of the way, let's see how React Native calls native methods. In this case, 
we can divide them into two categories: synchronous and asynchronous. First, let's try to see 
what happens if we call a synchronous method, such as mutliply:
A screenshot from Xcode debugger calling multiply on the JavaScript thread
A screenshot from Android Studio debugger calling multiply on the JavaScript thread
It gets called on the JavaScript thread. If you think about it, it makes sense, as we want to stop 
the execution on that thread and wait until this native function returns its value. As you can 
see, you have to be extra careful with synchronous functions. If you left this innocent-looking 
function being called on a JavaScript thread by mistake
@objc func multiply(_ a: Double, b: Double) -> NSNumber {
    Thread.sleep(forTimeInterval: 20) // Go to sleep
    return a * b as NSNumber
}
Thread.sleep doesn't look quite innocent, though
you would block all interactions on the JavaScript thread for 20 seconds, essentially freezing 
the whole app.
Guide: Understand the Threading Model of Turbo Modules and Fabric
The Ultimate Guide to React Native Optimization
108
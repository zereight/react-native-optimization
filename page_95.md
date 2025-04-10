class MyObject {
    deinit { print("Deallocated") }
}
let obj = MyObject()                        // Reference Count = 1
let unmanaged = Unmanaged.passRetained(obj) // Reference Count = 2
let object1 = unmanaged.takeRetainedValue() // Reference Count = 1
However, if you decrease the reference counter below 0, you will get a crash!
let obj = MyObject()                          // Reference Count = 1
let unmanaged = Unmanaged.passUnretained(obj) // Reference Count = 1 
(no change)
let object1 = unmanaged.takeRetainedValue()   // CRASH! 
A rule of thumb is to always match your retained and unretained calls. If you call passRetained, 
you should call takeRetainedValue afterward and vice versa. You can also call toOpaque() on 
the unmanaged objects to get its raw pointer, which can be later passed to C / C++.
unmanagedObject.toOpaque() // 0x000056076df5b1a0
Now you know the common memory management patterns used across languages. On top of 
that, you should now understand how Swift, Kotlin, and C++ manage memory. In the next 
chapter, we'll discover how to detect memory leaks using built-in profiler tools.
Guide: Understanding Native Memory Management
The Ultimate Guide to React Native Optimization
99
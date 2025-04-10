Since Kotlin doesn't have a built-in deterministic destructor or deinitializer, we need to use 
a cleanup lifecycle like AutoCloseable:
class MyClass : AutoCloseable {
    override fun close() {
        // Cleanup code here
        dataManager.unregisterListener(listener)
    }
}
We can also use WeakReference to store callbacks in DataManager class, which will 
automatically handle this. 
class DataManager {
    // List to store all registered listeners
    private val listeners = 
mutableListOf<WeakReference<DataListener>>()
    
    // ...
}
Using manual overrides
Languages that provide automated memory management usually also offer a manual way to 
alter the behavior, mostly for interoperability with external languages. While handy, when 
misused, this can be a hard-to-find source of memory leaks.
For example, in Swift, we can manually increment or decrement the reference count using the 
Unmanaged type. While doing so, you must be extra careful because it can lead to crashes and 
unexpected behavior. The Unmanaged class exposes a few functions that we can use to manage 
ARC behavior manually: 
 ■passRetained—creates an Unmanaged instance and increments the reference count.
 ■passUnretained—creates an Unmanaged instance without incrementing the reference 
count.
 ■takeRetainedValue—gets the object and decrements the reference count.
 ■takeUnretainedValue—gets the object without affecting the reference count.
Let's consider an example using Unmanaged on a Swift class (in most cases, you would use this 
to interface with C).
Guide: Understanding Native Memory Management
The Ultimate Guide to React Native Optimization
98
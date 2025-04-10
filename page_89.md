We can obtain a weak_ptr from a shared one; it doesn't increase the reference count. To use 
weak_pt, we have to use the .lock() method, which converts it to a shared pointer when the 
resource it points to still exists.
Android
On the Android platform, we mainly use Kotlin, which operates on  JVM (Java Virtual Machine) 
that uses Garbage Collection mechanisms to handle working with memory. Similarly to 
JavaScript (which also uses GC), we can introduce memory leaks by not freeing up resources. 
An example is forgetting to unregister listeners or cancel a long-running background operation. 
One of the few things we can do to instruct GC about managing memory is to use Weak types 
of containers, such as WeakHashMap (there is a similar construct in JavaScript called WeakMap). 
These types of containers don't create strong references to their items. Therefore, if nothing 
else is referencing them, the GC will deallocate those objects in its next pass. Here is an example 
usage of 
WeakHashMap:
fun main() {
   val weakMap = WeakHashMap<String, String>()
   
   // Add entries inside a scope
   run {
       weakMap[String("temp key")] = "some value"
       println("Map size: ${weakMap.size}") // Prints: 1
       println("Value: ${weakMap[key]}") // Prints: some value
   }
   
   // Force GC for demonstration
   System.gc()
   Thread.sleep(100)
   
   println("Map size after GC: ${weakMap.size}") // Prints: 0
}
What's important in the case of  WeakHashMap is that it keeps weak references only to its keys. 
If you need a  weak reference to the values stored in the map, you can use WeakReference() 
and assign the values as follows:
weakMap["key"] = WeakReference(LargeObject("1"))
iOS
As mentioned before, Swift and Objective-C use reference counting across the whole ecosystem. 
Let's explore this simple example to give you a better understanding of when the reference 
count is changed across scopes. We will use a Person class for the demonstration purposes:
Guide: Understanding Native Memory Management
The Ultimate Guide to React Native Optimization
98
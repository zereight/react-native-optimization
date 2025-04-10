an argument to a function. When the reference is terminated (e.g., going out of scope), the use 
count is decremented. If the use count reaches zero, the object is deallocated, and resources are 
freed. 
While it may impact runtime performance, it has the benefit of guaranteed cleanup and freeing 
the resources exactly when the last reference goes out of scope. Even though it seems like an 
ideal solution, we must be careful of "Reference Cycles", which happen when two objects hold 
"strong" references to each other. This essentially locks the reference counter at 1, preventing 
objects from being deallocated. A common solution for this issue is to use weak references, 
where there is a chance of introducing a reference cycle.
Garbage Collector
This pattern of managing memory is used in JavaScript, Java C#, and others.
Garbage Collector (GC) works independently from what's happening in your code. Think 
of it as a separate process, e.g., the Hermes engine spawns a separate thread where it runs its 
Hades GC. It can get invoked periodically or during one of the object allocations. Garbage 
Collector scans the program's memory and searches for objects that are no longer reachable. 
The unreachable objects are then freed. 
This approach has no problems with reference cycles. Still, the moment when the Garbage 
Collection process starts is not deterministic, and we have no control over when the objects 
will be destroyed. Also, if the GC algorithm is not concurrent in any way, our application is 
paused during the GC process, which can impact perceived user performance in some cases.
Manual management
Lower-level languages, like C or C++, allow us to take full control over allocations and 
deallocations of objects in memory. Typically, there are two places where you can allocate 
memory: stack and heap. 
The stack is a region of memory that stores data in a last-in-first-out (LIFO) manner, where 
memory is automatically allocated and deallocated. It's very fast, but limited in size and scope. 
The heap, on the other hand, is a larger region of memory used for dynamic allocation where 
data can be accessed globally. When you allocate memory on the heap (for example, using 
new 
or malloc), you are responsible for deallocating it when it's not needed anymore. 
This technique is the most efficient when done right; however, if you don't know what you are 
doing, you can easily shoot yourself in the foot.
Managing memory
Now that you know the theory, let's dive into practice! In this section, we will go over Kotlin, 
Swift, and C++ memory management to give you a deeper understanding of how to manage 
memory in each language properly.
Guide: Understanding Native Memory Management
The Ultimate Guide to React Native Optimization
94
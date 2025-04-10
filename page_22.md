GUIDE
HOW TO HUNT JS 
MEMORY LEAKS
When a program is executed on a target device or virtual machine, it always occupies some 
space in the device's Random Access Memory (RAM), which is allocated specifically for that 
program to execute correctly. When that memory allocation is unexpectedly retained, we say 
it's "leaking"; hence, we deal with memory leaks.
Memory leaks can be hard to trace without the right tools. In most cases, they arise from 
programmer errors. If your app constantly uses more and more memory without releasing 
it, you most likely have a leak somewhere. You might try to play the guessing game of what is 
leaking, but it's way easier to profile your app with the right tools that will help you find it. 
Let's start by defining what memory leaks are.
Anatomy of a memory leak in a JavaScript app
JavaScript engines, such as Hermes—and any other interpreted languages that leverage a virtual 
machine to execute—implement a special program called garbage collector (GC). As the name 
implies, it "collects garbage", where "garbage" refers to memory addresses that are no longer 
needed for the application to run and can be freed for other programs to use. GC periodically 
scans allocated objects and determines if an object is no longer needed. Fun fact: Hermes' 
garbage collector is called Hades.
Although garbage collectors use advanced techniques to reliably and safely determine which 
memory to free, in some cases, your code may sometimes trick the GC into keeping an object 
allocated when it shouldn't be. That's how you end up with memory leaks that are often hard 
to track without dedicated tools.
In short, a memory leak happens when a program fails to release the memory that 
is no longer needed.
Guide: How to Hunt JS Memory Leaks
The Ultimate Guide to React Native Optimization
24
GUIDE
UNDERSTANDING NATIVE 
MEMORY MANAGEMENT
In native programming languages, developers are usually more or less responsible for managing 
the memory and resources during the app's execution. The way we do this depends on the 
language and the platform. Memory management can be split into two main categories: manual 
and automatic.
When manually managing memory, e.g., in languages such as C or C++, we are responsible for 
allocating and freeing the memory (and other resources). This approach is generally verbose 
and error-prone; however, when done right, it can yield the best performance.
In most cases, you should choose automated memory management to make your code easier to 
understand and maintain. However, if you want to squeeze every nanosecond out of your code 
and have a deep understanding of how to manage memory, you will be better off with manual 
management. It's a common practice, for example, in game development. 
Knowing common patterns used by programming languages to manage memory is crucial 
because it allows you to reason about the code you write, making it more efficient and predictable. 
Let's start by exploring automatic memory management. 
Memory management patterns
In this approach, memory is managed automatically using a garbage collector algorithm. We 
can highlight two common approaches, each with its pros and cons. 
Reference counting
In mobile development, this pattern is used by Objective-C and Swift (under the name 
Automatic Reference Counting, or ARC), but it's also present in Python, PHP, and others.
Whenever we pass a reference to our object, the programming language's reference counter 
increments the number of its usages. This includes passing the object to another object or as 
Guide: Understanding Native Memory Management
The Ultimate Guide to React Native Optimization
93
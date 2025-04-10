bigger overhead than the stack. At the same time, you need to be cautious of how much you 
allocate on the stack, or else you risk... a stack overflow! But if possible, use stack and resort to 
allocating on the heap when necessary.
Reference cycles
The most common source of memory leaks when using reference counting are reference cycles, 
also known as circular references. Let's consider a simple example:
class A {
  std::shared_ptr<B> b;
};
class B {
  std::shared_ptr<A> a;
};
C++
class A {
 var b: B?
}
class B {
  var a: A?
}
Swift

In this example, class A depends on class B, which in turn depends on class A, causing a reference 
cycle.
It's worth noting that the reference cycles will be more complex in real-world 
scenarios; the cycles can span multiple classes, which makes them harder to find 
and fix.
The solution is to break the cycle by introducing a weak pointer (std::weak_ptr<T>). A weak 
pointer is an optional dependency that does not block the dependent object from being removed 
if it's no longer used. Whenever we want to access the dependency, we need to check if it still 
exists, as it is no longer guaranteed:
class A {
  std::shared_ptr<B> b;
};
class B {
  std::weak_ptr<A> a;
};
C++
class A {
 var b: B?
}
class B {
  weak var a: A?
}
Swift
Alternatively, we could introduce a third class, C, holding the shared resource that classes A and 
B depend on, which would help us avoid the cycle. This refactoring technique is commonly 
used in JavaScript codebases.
Not freed-up resources
In languages using garbage collector mechanisms, we can often introduce memory leaks by not 
freeing up resources. This can be observed when using the listener pattern and forgetting to 
call to remove them. If you come from a JavaScript background, you probably already know it 
all too well. Let's look at an example that showcases this common issue. First, create a simple 
DataManager class responsible for registering and calling listeners:

// Simple data manager class
interface DataListener {
    fun onDataChanged(data: String)
}
class DataManager {
    // List to store all registered listeners
    private val listeners = mutableListOf<DataListener>()
    
    // Method to register listeners
    fun registerListener(listener: DataListener) {
        listeners.add(listener)
    }
    
    // Method to unregister listeners
    fun unregisterListener(listener: DataListener) {
        listeners.remove(listener)
    }
    
    // Method to notify listeners of data changes
    fun notifyDataChanged(data: String) {
        listeners.forEach { it.onDataChanged(data) }
    }
}
Then, when we use this class, we have to remember about unregistering the listeners because 
otherwise, we will introduce memory leaks:
class MyClass {
    private val dataManager = DataManager()
    
    private val listener = object : DataListener {
        override fun onDataChanged(data: String) {
            println("Data changed: $data")
        }
    }
    
    init {
        // Register listener but never unregister
        dataManager.registerListener(listener)
    }
    
    // Memory leak: No way to unregister the listener
    // MyClass instance will never be garbage collected
    // as DataManager holds a strong reference to the listener
}

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
to interface with C / C++).

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

GUIDE
UNDERSTAND THE 
THREADING MODEL OF TURBO 
MODULES AND FABRIC
Understanding how React Native executes your code is crucial for building efficient native 
modules. In this chapter, we will walk you through the lifecycle of a native module, from 
the initialization phase to calls and de-initialization. By the end of this chapter, you should 
understand which thread is used under what circumstances.
Threading model
Let's start by exploring what threads are available to us in a mobile React Native app:
 ■Main thread / UI thread—whether it's a React Native or plain native app, this thread 
handles UI operations and maintains a responsive user experience.
 ■JavaScript thread—as the name suggests, it's used (although not exclusively) to execute 
JavaScript code).
 ■Native modules thread—a shared pool allocated by React Native specifically for native 
modules.
In addition, you, React Native's renderer, and the third-party modules can create additional 
threads to handle tasks in the background. You can learn more about this topic in the Make 
Your Native Modules Faster chapter.
Turbo Modules
To better understand the threading model of Turbo Modules, let's examine its lifecycle by 
checking how and on which thread a given method is called. 

First, let's check where the init function will be called on iOS:
A screenshot from Xcode debugger calling init on the main thread
and on Android:
A screenshot from Android Studio debugger calling init on the JavaScript thread
There is a notable discrepancy between platforms. On Android, the init method was called 
on the JavaScript thread, called mqt_v_js, while iOS used the main thread. The difference 
comes from an assumption made in React Native about modules overriding the init method. 
Whenever we override the init function in an iOS app, React Native assumes that we may 
access UIKit; therefore, it calls the function on the main queue (UIKit is not thread-safe).
React Native source code explaining running init on the main thread

After removing this assumption, the module is initialized on the JavaScript thread, 
which follows the same behavior as on Android. Altering React Native internals 
requires building it from the source. While this is not recommended for production 
apps, it can be a valuable learning experience in a development environment.
Next, let's see if opting out of lazy initialization changes the thread our module is using. This 
feature is available only on Android. To opt in to eager loading, open your library's package file 
and change the fourth parameter needsEagerInit to true:
class AwesomeLibraryPackage : BaseReactPackage() {
  // Other code
  
  override fun getReactModuleInfoProvider(): ReactModuleInfoProvider 
{
    return ReactModuleInfoProvider {
      val moduleInfos: MutableMap<String, ReactModuleInfo> = 
HashMap()
      moduleInfos[AwesomeLibraryModule.NAME] = ReactModuleInfo(
        AwesomeLibraryModule.NAME,
        AwesomeLibraryModule.NAME,
        false,  // canOverrideExistingModule
        true,   // needsEagerInit <- Change this to true
        false,  // isCxxModule
        true    // isTurboModule
      )
      moduleInfos
    }
  }
}
Creating CoroutineScope in a Kotlin file
Let's see what happens after changing the flag and re-building the app:
A screenshot from Android Studio debugger calling init on the Turbo Modules thread

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
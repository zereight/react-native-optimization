class MainApplication : Application(), ReactApplication {
  private fun isForegroundProcess(): Boolean {
    val processInfo = ActivityManager.RunningAppProcessInfo()
    ActivityManager.getMyMemoryState(processInfo)
    return processInfo.importance == IMPORTANCE_FOREGROUND
  }
}
You can access the app initialization timestamp using the following APIs on iOS and Android 
to get data for the nativeLaunchStart metric:
var tp = timespec()
clock_gettime(CLOCK_THREAD_CPUTIME_ID, &tp)
iOS nativeLaunchStart
Process.getElapsedCpuTime()
Android nativeLaunchStart
Then, the closest approximation of nativeLaunchEnd would be when a native module is 
initialized on iOS and when Android Content Provider is created on Android:
+ (void) initialize
iOS nativeLaunchEnd
class StartTimeProvider : ContentProvider() {
    override fun onCreate(): Boolean {}
}
Android nativeLaunchEnd
Initializers and other pre-main steps are run preemptively, potentially hours before the iOS 
app is started and main() is run. So, take the difference between process start time in pre-main 
initializer into account.
Native App Init
To hook into the time when the iOS app is created and get the appCreationStart marker, 
you can hook into the main method. For Android, you can leverage the  Main Application's 
onCreate lifecycle method:

int main(int argc, char *argv[])
iOS appCreationStart
class MainApplication : Application(), ReactApplication {
    override fun onCreate() {}
}
Android appCreationStart
As prewarming on iOS executes an app's launch sequence up until, but not including, the time 
when main() calls UIApplicationMain, it's done until this point and we're safe from its effects 
on timing.
Then, the native app creation process ends with didFinishLaunchingWithOptions on iOS 
and the onStart lifecycle method on Android, where we can hook our appCreationEnd logic:
func application(_ application: UIApplication, 
didFinishLaunchingWithOptions launchOptions: [UIApplication.
 LaunchOptionsKey: Any]?) -> Bool
iOS appCreationEnd
class MyApp : Application() {
    override fun onStart() {}
}
Android appCreationEnd
JS Bundle Load
Next, we can finally access a React Native-related metric, runJSBundleStart. On iOS, we can 
tap into the global notification center listening for "RCTJavaScriptDidLoadNotification" 
event from the React Native's core, and on Android, we can import ReactMarker  from  the  
core to listen for that event:
NotificationCenter.default.addObserver(self,
    selector: #selector(emit),
    name: NSNotification.Name("RCTJavaScriptDidLoadNotification"),
    object: nil)
iOS runJSBundleStart

ReactMarker.addListener { name ->
    when (name) { RUN_JS_BUNDLE_START -> {} }
}
Android runJSBundleStart
In a similar fashion, we can listen to the end of the JS bundle loading to get our runJSBundleEnd 
marker:
NotificationCenter.default.addObserver(self,
    selector: #selector(emit),
    name: NSNotification.Name("RCTJavaScriptDidLoadNotification"),
    object: nil)
iOS runJSBundleEnd
ReactMarker.addListener { name ->
    when (name) { RUN_JS_BUNDLE_END -> {} }
}
Android runJSBundleEnd
React Native Root View Render
We can reuse the same React Marker infrastructure to detect when the React Native content 
appears and mark our contentAppeared marker:
NotificationCenter.default.addObserver(self,
    selector: #selector(emitIfReady),
    name: NSNotification.Name("RCTContentDidAppearNotification"),
    object: nil)
iOS contentAppeared
ReactMarker.addListener { name ->
    when (name) { CONTENT_APPEARED -> {} }
}
Android contentAppeared
React App Render
Our last marker, screenInteractive, will give us the overall TTI metric. There's no single 
place we can define this marker. It's specific to each application, and you'll need to decide when 
the user can safely interact with the contents of your app. As a base, you can put it in the initial 

mount of the main useEffect of, for example, a home screen. Eventually, find a better spot, 
such as when a top-level content of your app, that users typically start with, is displayed:
export default HomeScreen() {
  useEffect(() => {}, [])
  return <TabNavigator {...} />
}
screenInteractive marker in React app
Putting it all together, you'll have a bird's eye view of the whole initialization pipeline of your 
app—from the native init to the screen, or ideally, multiple different screens, to being interactive. 
You can send this data to a database or a real-time user metrics platform, observe how your 
app's performance is affected on various devices, and—what's even more important—how it 
changes over time. Having this data, you'll be able to make exact correlations between TTI, 
and your app's customer success metrics. This data can and should influence the priorities 
of your engineering team, giving you signs that it's either time to optimize or cut back on 
optimizations that are not effective anymore and move focus somewhere else.

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

C++
In C++ programming language, memory management is explicit and manual. However, long 
gone are the times when you had to manually call the new and delete operators for all memory 
management. Instead, you can use smart pointers, which the C++ standard library (called std) 
provides. This way, you don't have to manually allocate and deallocate memory, resulting in 
fewer memory leaks in your application.
Further C++ materials assume some knowledge about the concept of pointers in 
C and C++ languages. If you're not familiar with it, feel free to learn more or skip 
this part.
There are multiple smart pointers available at your disposal: 
 ■std::unique_ptr<T>—it cannot be copied to another unique_ptr, passed by value to 
a function, or used in any C++ Standard Library algorithm that requires copies to be 
made. A unique_ptr can only be moved.
 ■std::shared_ptr<T>—reference-counted smart pointer. Use it when you want to 
assign one raw pointer to multiple owners, for example, when you return a copy of a 
pointer from a container but want to keep the original.
 ■std::weak_ptr<T>—special-case smart pointer for use in conjunction with shared_
ptr. A weak_ptr provides access to an object owned by one or more shared_ptr 
instances but does not participate in reference counting.
The most commonly used smart pointer is a std::shared_ptr<T>, which is a wrapper object 
providing a reference counting mechanism for the specified type, a memory management 
mechanism known from Obj-C and Swift. Let's go over an example usage of smart pointers: 
Let's start with std::unique_ptr:
void takeOwnership(std::unique_ptr<std::string> s1) {
    std::cout << *s1;
    // Gets automatically deleted when function ends
}
int main() 
{
    auto str1 = std::make_unique<std::string>("Hello World");
    
    std::cout << *str1;
    
    // Can only be moved. Doesn't allow for copying
    takeOwnership(std::move(str1));
    
    // str1 is cleared here
    
    return 0;
}
In the example above, we create a new unique pointer holding a std::string  using  make_
unique. Then, using dereferencing, we can print out the value of the string. Additionally,  the 
example shows the uniqueness of this smart pointer by transferring the ownership to a different 
function, which can only be done by moving. This is useful when you want to have exclusive 
ownership over the pointer for example when building immutable data structures.
Now, let's explore std::shared_ptr. We'll use the same example to make things easier to 
understand:
void takeOwnership(std::shared_ptr<std::string> s1) {
    std::cout << *s1;
    // After function returns, reference count gets back to 1
}
// We accept a reference
void takeReference(const std::shared_ptr<std::string> &s1) {
  std::cout << *s1;
}
int main() 
{
      // Create a shared pointer
    auto str1 = std::make_shared<std::string>("Hello World");
    
    std::cout << *str1;
    
    // We increase the reference count by 1 and create a copy of 
    the pointer (not underlying values)
    takeOwnership(str1);
    
    // Since we didn't move the object it can be still accessed 
    here
    std::cout << *str1;
    
    // We don't alter the reference count since we pass a reference 
    here
    // This doesn't copy anything
    takeReference(str1);
    
    std::cout << *str1;
    
    return 0;
}

In the example above, we create a new shared pointer using std::make_shared(), which can 
be printed out using dereferencing. We pass it to the takeOwnership function, which passes the 
shared pointer by value (makes a copy of the pointer, not the underlying value). This operation 
increases the reference counter. 
Since we can copy shared pointers, the value is not cleared, and we can still use it. Then, we 
pass it to the takeReference function, which, in contrast, doesn't increment the reference 
count since we only take the reference to this function. You should pass by reference when the 
function's use of the shared_ptr doesn't outlive the caller and pass by value when you need 
the `shared_ptr` to survive independently of the caller.
Now let's see how to use std::weak_ptr:
void useWeakPtr(std::weak_ptr<std::string> weak) {
    // Try to get access to the object
    // .lock() converts weak to shared_ptr if object exists
    if (auto shared = weak.lock()) {  
        std::cout << *shared;         // Safe to use
        // shared_ptr reference count temporarily increases here
    } else {
        std::cout << "Object no longer exists!\n";
    }
}  // shared goes out of scope, count decreases
int main() {
    // Create a shared pointer
    auto str1 = std::make_shared<std::string>("Hello World");
    
    // Create a weak pointer from shared pointer
    std::weak_ptr<std::string> weak1 = str1;  // Doesn't increase 
reference count
    
    std::cout << "Reference count: " << str1.use_count() << "\n";  
// Shows 1
    
    // Use the weak pointer
    useWeakPtr(weak1);  // Object exists
    
    // Reset the shared pointer
    str1.reset();  // Reference count becomes 0, object is 
destroyed
    
    // Try to use weak pointer again
    useWeakPtr(weak1);  // Will print "Object no longer exists!"
    
    return 0;
}

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

class Person {
    let name: String
    
    init(name: String) {
        self.name = name
    }
}
do {
    let person1 = Person(name: "John") // Reference Count: 1
    
    do {
        let person2 = person1  // Reference count: 2
    } // person2 goes out of scope, Reference count: 1
    
} // person1 goes out of scope, Reference count: 0
When we assign person1 to person2, we increase the reference count (we do not create a copy!). 
Then, after we go out of the do block scope, the reference count is set back to 1 and then to 0.
Sources of Memory Leaks
Let's examine some common patterns that cause our app to leak memory.
Forgetting to deallocate in manual management
When using manual memory management, it's easy to introduce a leak. As mentioned before, 
if you choose manual management, you are on your own. With great power comes great 
responsibility. To show you how easy it is to introduce a leak, let's consider this example:
int main() 
{
    std::string *str1 = new std::string {"Hey"};
    
    std::cout << *str1;
    
    // Oops! Memory leak, we forgot to delete str1.
    
    return 0;
}
You allocate a new std::string, print its value, and, at the end, we forgot to delete this string; 
therefore, we introduced a memory leak. To fix it, you need to call delete when a resource is 
not needed:
int main() 
{
    std::string *str1 = new std::string {"Hey"};
    // ...
    delete str1;
 
    return 0;
}
Another option would be to allocate this on the stack (dropping the new), which will 
automatically deallocate str1 when it goes out of scope:
int main() 
{
    std::string str1 = std::string {"Hey"};
    
    cout << str1;
    
    return 0;
}
You should always think twice before putting something on the heap, as it has a significantly
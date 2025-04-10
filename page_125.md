To create a coroutine, we'll use the CoroutineScope class and instantiate it in the moduleScope 
variable:
@ReactModule(name = AwesomeLibraryModule.NAME)
class AwesomeLibraryModule(reactContext: ReactApplicationContext) :
  NativeAwesomeLibrarySpec(reactContext) {
  // Other methods..
 +  private val moduleScope = CoroutineScope(Dispatchers.Default + 
SupervisorJob())
 +  override fun invalidate() {
 +    super.invalidate()
 +    moduleScope.cancel()
 +  }
}
Creating CoroutineScope in a Kotlin file
Remember to cancel the scope when the module gets invalidated. This makes sure that we don't 
introduce any memory leaks. Now that we have the moduleScope available, we can offload the 
work to a background thread using its launch method:
override fun multiplyOnBackgroundThread(a: Double, b: Double, 
promise: Promise?) {
  moduleScope.launch {
    promise?.resolve(a * b)
  }
}
Example use of moduleScope.launch in Kotlin
Upon calling this function, once you place a debugger breakpoint, you can observe that the 
multiplyOnBackgroundThread is being run on a worker thread instead of the main thread.
A screenshot from Android Studio stopped on a breakpoint
Best Practice: Make Your Native Modules Faster
The Ultimate Guide to React Native Optimization
128
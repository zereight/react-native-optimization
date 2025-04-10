After creating the module, first enable Swift support by modifying your library Podspec file 
that comes from CocoaPods:
- s.source_files = "ios/**/*.{h,m,mm,cpp}"
+ s.source_files = "ios/**/*.{h,m,mm,cpp,swift}"
Changes in the Podspec necessary for CocoaPods to understand Swift as a source to link
Next, create a new Swift file from within Xcode. Make sure to place it inside your module's 
iOS folder. Click "yes" when asked if you want to create a Bridging Header.
Next, add these changes inside of your library header file:
#import <Foundation/Foundation.h>
+ #if __cplusplus
#import "ReactCodegen/RNAwesomeLibrarySpec/RNAwesomeLibrarySpec.h"
+ #endif
@interface AwesomeLibrary : NSObject
+                                      #if __cplusplus
                                       <NativeAwesomeLibrarySpec>
+                                      #endif
@end
A diff of changes necessary for Swift to not fail on unsupported C++ types.
Swift doesn't understand C++ types, so we use #if   __cplusplus macro to import the spec 
file only if __cplusplus is defined (which is not for Swift). Next, inside your Bridging Header, 
import your main library header file:
+ #import "AwesomeLibrary.h"
Now, we can create an extension to our AwesomeLibrary class to make it available to 
Objective-C through @objc attribute:
import Foundation
extension AwesomeLibrary {
  @objc func multiply(_ a: Double, b: Double) -> NSNumber {
    return a * b as NSNumber
  }
}
AwesomeLibrary extension with multiply method exposed to Objective-C

What's left is to use the method we just implemented. To do so, navigate to your library .mm file 
(the one handling Objective-C++ syntax) and use RCT_EXTERN_METHOD():
#import "AwesomeLibrary.h"
#if __has_include("awesome_library/awesome_library-Swift.h")
#import "awesome_library/awesome_library-Swift.h"
#else
#import "awesome_library-Swift.h"
#endif
@implementation AwesomeLibrary
RCT_EXPORT_MODULE()
RCT_EXTERN_METHOD(multiply:(double)a b:(double)b);
// rest of the module
@end
Let's break down what happens here. First, we import our module. Xcode generates a header 
file called <library-name>-Swift.h . It's prefixed with the library name when we are running 
in static/dynamic frameworks mode, which is why we check if this import exists. If it doesn't, 
we fall back to a normal one.
Next, we use RCT_EXTERN_METHOD, which remaps the Objective-C function to our Swift call. 
That's all we need to use Swift inside our module. There are ongoing efforts to support Swift 
by default, so hopefully, this boilerplate won't be necessary in the future.

Leverage background threads
In a browser context, we typically deal with one thread. We can, of course, offload some JavaScript 
work to a web worker, but it's a bit different. In React Native, we can leverage the power of 
devices in the user's hands. By default, synchronous Turbo Module methods get called on the 
JavaScript thread, as you can see below on iOS:
A screenshot from Xcode stopped on a breakpoint

The same happens on Android:
A screenshot from Android Studio stopped on a breakpoint
The deep dive into threading in a React Native app is covered in the Understand 
the Threading Model of Turbo Modules and Fabric chapter.
Now, we want to use that multithreading and schedule some work to be done on a background 
thread. Let's introduce a new asynchronous method multiplyOnBackgroundThread. To keep 
this guide concise, we will skip the details on adding a new method and focus on offloading 
code to a background thread.
Let's start with iOS. To run our code on a background thread, we can use a higher-level concept 
of queues, available in the form of DispatchQueue API, which manages the scheduling and 
execution of tasks, ensuring they're not run on the main thread. The code available in the 
DispatchQueue.global() block will be executed once the global concurrent queue for 
background work is available. Inside the block, we can call the resolve() method to notify 
our JS function about the result.
 @objc func multiplyOnBackgroundThread(
    _ a: Double,
    b: Double,
    resolve: @escaping RCTPromiseResolveBlock,
    reject: RCTPromiseRejectBlock
  ) {
    DispatchQueue.global().async {
      resolve(a * b)
    }
  }
Example use of DispatchQueue.global() in Swift
Let's see how to achieve the same thing on Android. Much like with Swift, we won't access 
a background thread directly. Instead, we'll use coroutines. They are a part of Kotlin's 
concurrency framework and provide a way to write asynchronous code that is both readable and 
maintainable. It's worth noting that coroutines are not bound to a physical thread—thousands 
of coroutines can run on a small number of threads.

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

The same thing happens on iOS:
A screenshot from Xcode stopped on a breakpoint
There are a few more caveats about threading, which we discuss in the Understand 
the Threading Model of Turbo Modules and Fabric chapter.
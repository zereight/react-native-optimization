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
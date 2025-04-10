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
Swift doesn't understand C++ types, so we use #if   __cplusplus macro to import the spec 
file only if __cplusplus is defined (which is not for Swift). Next, inside your Bridging Header, 
import your main library header file:
+ #import "AwesomeLibrary.h"
Now, we can create an extension to our AwesomeLibrary class to make it available to 
Objective-C through @objc attribute:
import Foundation
extension AwesomeLibrary {
  @objc func multiply(_ a: Double, b: Double) -> NSNumber {
    a * b as NSNumber
  }
}
AwesomeLibrary extension with multiply method exposed to Objective-C
Best Practice: Make Your Native Modules Faster
The Ultimate Guide to React Native Optimization
124
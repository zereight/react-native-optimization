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
Guide: Understand the Threading Model of Turbo Modules and Fabric
The Ultimate Guide to React Native Optimization
107
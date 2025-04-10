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
Guide: How to Measure TTI
The Ultimate Guide to React Native Optimization
89
On Android, you'll need to tap into the onActivityCreated lifecycle method to determine 
that information, so it's a bit more verbose:
class MainApplication : Application(), ReactApplication {
  var isColdStart = false
  override fun onCreate() {
    super.onCreate()
    var firstPostEnqueued = true
    Handler().post {
      firstPostEnqueued = false
    }
    registerActivityLifecycleCallbacks(object : 
ActivityLifecycleCallbacks {
      override fun onActivityCreated(
        activity: Activity,
        savedInstanceState: Bundle?
      ) {
        unregisterActivityLifecycleCallbacks(this)
        if (firstPostEnqueued && savedInstanceState == null) {
          isColdStart = true
        }
      }
    })
  }
}
Since we want to measure TTI only when the app is in the foreground, we'll need to account 
for that. In AppDelegate.swift file on iOS, we can hook into the  didFinishLaunchingWith 
Options handler:
@main
class AppDelegate: RCTAppDelegate {
  var isForegroundProcess = false
 override func application(_ application: UIApplication, 
didFinishLaunchingWithOptions launchOptions: [UIApplication.
 LaunchOptionsKey : Any]? = nil) -> Bool {
    if application.applicationState == .active {
      isForegroundProcess = true
    }
    return true
  }
}
And  in  MainApplication.kt file on Android, we can check that through processInfo.
importance API that we can wrap in a helper isForegroundProcess function:
Guide: How to Measure TTI
The Ultimate Guide to React Native Optimization
88
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
Guide: How to Measure TTI
The Ultimate Guide to React Native Optimization
90
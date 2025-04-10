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
Guide: How to Measure TTI
The Ultimate Guide to React Native Optimization
91
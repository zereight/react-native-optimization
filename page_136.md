+        noCompress += ["bundle"]
 +    }
  }
app/build.gradle file with compression disabled for .bundle files
As a result, the index.android.bundle file won't be compressed, effectively increasing the 
overall app bundle size. This size increase will affect the install size of your app but not the 
download size. The most important bit is that your app should load significantly faster now 
due to Hermes being able to leverage memory mapping and load the JS bundle faster. In our 
initial testing, for an APK of 75.9 MB install size, it was a 6.1 MB size increase (+8%), while the 
TTI dropped by 450 ms (-16%).
It's possible, but not very likely, that your app won't grow in size for various reasons. Measure 
the outcomes and observe your users' behavior to ensure this change is a net positive rather 
than a net negative. Most likely, though, the benefits will outweigh the cost.
Best Practice: Disable JS Bundle Compression
The Ultimate Guide to React Native Optimization
176
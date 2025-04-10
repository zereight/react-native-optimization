As a result, when that build phase gets executed, Metro will place assets inside RNAssets.
xcassets catalog. After running the build for the first time, your asset folder should look 
like this:
A look into Xcode Asset Catalog with sample images
You can also trigger the bundle command manually if you run into issues or want to test if 
everything works (notice that --asset-catalog-dest now points to ios because we run this 
script from the root of the React Native project):
> npx react-native bundle \
  --entry-file index.js \ 
  --bundle-output output.js \ 
  --platform ios \
  --dev false \
  --minify true \ 
  --asset-catalog-dest ios \
  --assets-dest <your-assets-folder>
Now, after uploading your app to the App Store, the app thinning mechanism will kick in and 
choose proper image sizes based on the user's device!
On iOS, "App Thinning" optimizes app delivery by creating device-specific 
versions, including only required assets. Android achieves similar results through 
"Android App Bundles" (AAB), which generate optimized APKs for each device 
configuration, breaking apps into modular components that can be downloaded 
on demand. 
Testing it out
Let's examine the real difference on iOS, before and after applying this optimization. Without 
it, your bundle will include all three images, which is a waste because your device will only use 
one size.
Best Practice: Use Native Assets Folder
The Ultimate Guide to React Native Optimization
173
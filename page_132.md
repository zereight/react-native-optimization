Xcode Asset Catalog
We can achieve similar results on iOS. However, unlike on Android, it is not done automatically 
by React Native and Metro while creating the bundle. In order to benefit from similar 
optimization, we must explicitly create an Xcode Asset Catalog (.xcassets) and configure it as 
part of our compilation pipeline.
To get started, let's create the RNAssets.xcassets file and place it inside the ios folder.
This folder has to be named RNAssets.xcassets due to the hard-coded path 
inside React Native bundle scripts. Hopefully, this constraint will be dropped in 
the future.  
After creating the folder, navigate to Build Phases from within your Projects Settings and 
modify the Bundle React Native code and images build phase with EXTRA_PACKAGER_
ARGS variable above line 8:
export EXTRA_PACKAGER_ARGS="--asset-catalog-dest ./"
Bundling code and images happens as one of build phases
Best Practice: Use Native Assets Folder
The Ultimate Guide to React Native Optimization
172
> npm install react-native-bottom-tabs
This command fetches the package from the npm registry, adds it to node_modules/,  and  
updates package.json.
Here are some files to look out for:
 ■package.json—defines project dependencies, scripts, and metadata.
 ■node_modules—stores all installed dependencies locally.
 ■package-lock.json—ensures version consistency across environments.
 JavaScript developers often use alternative package managers, such as Yarn, PNPM, 
or Bun.
 iOS
For iOS, React Native uses CocoaPods to manage native dependencies.
Unlike a traditional iOS project, where dependencies are fetched from CocoaPods (Podspec 
repositories) or Swift Package Manager, React Native libraries typically ship their native code 
to the npm registry, and CocoaPods uses local references to locate and load that code. However, 
if you need to add a regular iOS-native dependency that is not a React Native library, you will 
need to fetch it from CocoaPods.
For example, to add SDWebImage to your project, you have to add the following to your Podfile:
pod 'SDWebImage', '~> 5.0'
Let's examine how package management works for JavaScript, iOS, and Android. Understanding 
this process will help you navigate your dependencies and pull in additional native modules 
when needed.
JavaScript
For JavaScript dependencies, React Native primarily relies on the npm registry, a centralized 
repository for JavaScript libraries. This registry contains packages that may include only 
JavaScript code or a combination of JavaScript and native code.
Installing a JavaScript dependency in a React Native project is straightforward using npm:
Guide: Understand Platform Differences
The Ultimate Guide to React Native Optimization
69
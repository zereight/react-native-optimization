> cd ios && bundle exec pod install
Note that we're not running CocoaPods directly, but instead using the bundle command. Since 
CocoaPods is a Ruby project, we can leverage Bundler, Ruby's dependency manager, to have 
the most up-to-date CocoaPods version required for a given React Native project, specified in 
a top-level 
Gemfile that comes by default with every new React Native project. 
After installing pods, you can quickly launch Xcode using the xed . command. 
Guide: Understand Platform Differences
If you created a new React Native project with React Native Community CLI, the result may 
look as follows:
require_relative '../node_modules/react-native/scripts/react_
native_pods'
require_relative '../node_modules/@react-native-community/cli-
platform-ios/native_modules'
platform :ios, '12.4'
install! 'cocoapods', :deterministic_uuids => false
target 'HelloWorld' do
  # Here goes your native dependency
  pod 'SDWebImage', '~> 5.0'
  
  # Rest of Podfile
end
This is a typical Podfile from a fresh React Native project with some parts removed for readability;  
you can see the full contents of this Podfile here
The ~> operator in CocoaPods follows a pessimistic version constraint similar 
to SemVer (Semantic Versioning), but with a slight difference:
 ■ ~> 5.0 allows versions >= 5.0 but < 6.0 (patch and minor updates allowed, 
but no major version updates).
 ■~> 5.0.1 allows versions >= 5.0.1 but < 5.1 (only patch updates allowed, 
no minor version updates).
Once you've added your dependencies, go to the ios directory and install pods using the pod 
install command:
The Ultimate Guide to React Native Optimization
70
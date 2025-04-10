Removing these polyfills alone shaves off over 430 kB from our JS bundle. This code is loaded 
eagerly at our app's entry point, so it can potentially improve the app's TTI—all by removing 
unnecessary code.
Crypto libraries
Another example worth considering is replacing libraries related to heavy computation with 
their native substitutes. One example is a JavaScript implementation of the Node.js crypto 
library called crypto-js, which can be replaced with react-native-quick-crypto  from  
Margelo   . This replacement can yield results up to 58x faster due to using C++ directly.
This is extremely important for projects that rely on random numbers and need to use 
cryptographically secure pseudorandom number generator (CSPRNG), for example, to 
generate a Web3 Wallet seed. This function can't be implemented relying on JavaScript's 
Math.random() because it can't guarantee that the result will contain enough entropy to be 
considered secure.
You can read more about this in this blog post about increasing speed and security with native 
crypto libraries.
Use native navigation
Most apps need a navigation solution; one of the most popular is React Navigation. It's a robust 
library allowing for a mix of JS and native navigators, depending on your design needs and 
performance requirements. While setting up your navigation structure, you might wonder 
which stack navigator to choose: JS Stack with ultimate flexibility or Native Stack with less 
flexibility but better performance and native look. It's a similar story if you use tab navigator: 
should you go with JS Tabs and Native Tabs, which have similar trade-offs as stack navigator? 
There are multiple benefits to choosing native-based navigators. First and foremost, they unload 
the work from the JavaScript thread, making your app use less memory and be smoother overall. 
There is also a really important factor: native feel. Both Native Stack and Native Tabs give your 
app extra-level polish by using native platform primitives used by other apps on this platform. 
Native Stack Navigator
To use the native stack navigator from react-native-screens project with React Navigation, 
all you need to do is install and use the @react-navigation/native-stack package like this:
import { createNativeStackNavigator } from '@react-navigation/
native-stack';
const MyStack = createNativeStackNavigator({
  screens: {
    Home: HomeScreen,
    Profile: ProfileScreen,
  },
});
Initializing native stack navigator
Best Practice: Use Dedicated React Native SDKs Over Web
The Ultimate Guide to React Native Optimization
119
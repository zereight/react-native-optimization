Native Android header with back button from react-
native-screens
 Stack Navigator
Its API is very similar to the JavaScript stack navigator, so migrating it is often not complicated.
Large iOS header from react-native-screens Stack 
Navigator
Native Tabs Navigator
Similarly, instead of using a built-in JS navigator available in React Navigation, you can use the 
react-native-bottom-tabs project, which provides a mostly compatible API. 
Similarly to Native Stack, you can replace your JavaScript-based navigation and use the same 
API:
import { createNativeBottomTabNavigator } from '@bottom-tabs/react-
navigation';
const MyTabs = createNativeBottomTabNavigator({
  screens: {
    Home: HomeScreen,
    Profile: ProfileScreen,
  },
});
Best Practice: Use Dedicated React Native SDKs Over Web
The Ultimate Guide to React Native Optimization
120
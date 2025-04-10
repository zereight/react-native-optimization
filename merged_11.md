A screenshot from Xcode debugger calling invalidate on the Turbo Modules thread
A screenshot from Android Studio debugger calling invalidate on pool-2-thread-1 which is "ReactHost" Thread
We have another small discrepancy between platforms. On iOS, the function is called on the 
Turbo Modules thread, and on Android, the thread doesn't have an explicit name; it's probably 
auto-generated. Going through the source code, you can find that it's a thread spawned by the 
ReactHost class. Now that you better understand the threading model—what is called on which 
thread and why—you'll be able to add advanced, thread-safe, and performant native functionalities to 
your React Native app.

BEST PRACTICE
USE VIEW FLATTENING
React component API is designed to be declarative and reusable through composition. We 
combine multiple components to create complex layouts, much like LEGO blocks. It works 
great on the web, where creating <div/> is quite cheap. React Native operates on native layout 
primitives, which are typically more expensive to process than web equivalents. To mitigate 
this issue, React Native introduced an optimization called "view flattening" to its core renderer. 
It simplifies the view hierarchy where possible to avoid extra memory pressure and CPU 
processing.
This kind of optimization was first introduced for the Android React Native 
renderer.  However,  as  the  project  progressed  with  the  New  Architecture, 
essentially rewriting core parts to C++, it made sense to move this platform-
specific optimization to the shared renderer. Thanks to that, view flattening is 
now possible on iOS too.
Without going into the details of how this works, the renderer identifies "layout-only" nodes. 
They only affect the layout of a 
View and don't render anything on the screen. Those elements 
can be flattened, leading to a depth reduction of the host elements tree. For an in-depth 
explanation of how the view flattening works, check out the official documentation.
Beware of issues with View Flattening
This optimization technique works great in most cases, but sometimes, we want our view 
to stay in the hierarchy. A common case for such a requirement is when you are working on 
a native UI component that accepts children.

<MyNativeComponent>
  <Child1/>
  <Child2/>
  <Child3/>
</MyNativeComponent>
MyNativeComponent with 3 child views
In the example above, the MyNativeComponent can expect to receive an array of 3 views on 
the native side. But what happens if view flattening decides that the Child1's container view 
is "layout-only"? You can unexpectedly get all of the children of this view passed to the native 
side. This means that if Child1 had 3 child views inside, you would get 5 children passed to 
your native component instead of 3. To illustrate that:
<MyNativeComponent>
  /* Child1 unexpectedly flattened to 3 Views */
  <View/> 
  <View/>
  <View/>
  <Child2/>
  <Child3/>
</MyNativeComponent>
Greedy view flattening
This is unexpected behavior if you assume the number of JavaScript children will match the 
children's array on the native side. As you can see, it's not a safe assumption and may prompt 
you to validate your component logic. But sometimes, you really need to know the children 
count. To solve it, you can use the collapsable prop, which controls view flattening behavior. 
Setting it to false will prevent the view from being merged with others.
<MyNativeComponent>
  <Child1 collapsable={false} />
  <Child2 collapsable={false} />
  <Child3 collapsable={false} />
</MyNativeComponent>
MyNativeComponent with 3 child views with collapsable prop set to false
After applying this prop, we can be sure that we will always receive 3 child views in our native 
component.

Debugging the view hierarchy
While working on view flattening, you might find view hierarchy debugging useful. Thankfully, 
you can inspect it on both iOS and Android platforms with Xcode and Android Studio, 
respectively.
Xcode
When you run your app through Xcode, you'll notice a debugging toolbar which, on top of 
standard breakpoint debugging, offers Hierarchy inspector. You can launch it by clicking the 
"Debug View Hierarchy" button:
Debug View Hierarchy button in Xcode
This stops your app, as it was stopped on a breakpoint, but now you can inspect the native 
view hierarchy:
View hierarchy of a sample React Native app; it really is a native app!
Seeing a visual representation of the view hierarchy can be helpful when solving layout-
related issues. If you look at the left sidebar, it lists the native views corresponding to 
JavaScript components, so a JavaScript <View/>  is  RCTViewComponentView, the same 
native building block as inside of fully native apps.

Android Studio
Same as with Xcode, we have a way to inspect the view hierarchy of our app; in Android, it's 
called "Layout Inspector". To launch it, navigate to the top menu bar and open View > Tool 
Windows > Layout Inspector.
Layout Inspector in Android Studio
Here, you can see a similar UI to what we have in Xcode. In the sidebar, there is a list of elements 
currently visible on the screen. You can notice that on this platform, <View/> from JavaScript 
corresponds to ReactViewGroup that holds ReactTextViews inside.
Being aware of how view flattening works is a useful skill. Most of the time, you shouldn't 
think about it as the idea behind it was to be fully transparent. Sometimes, however, you might 
run into issues where this optimization was misapplied, and the layout debugger should help 
you understand what's happening. And, if necessary, skip flattening of problematic views.

BEST PRACTICE
USE DEDICATED REACT 
NATIVE SDKS OVER WEB
One of the best things about React Native is that you can use all the good parts of the 
JavaScript ecosystem in mobile, tablet, and desktop apps. This means reusing some of your 
React components and managing the business logic state with your favorite library.
While React Native provides web-like functionality for compatibility with the web, you need 
to understand it's not the same environment. It has its own set of best practices, quick wins, 
and constraints. 
Using a dedicated platform-specific version of the library
The fact that it's often (but not always) possible to run the same JavaScript in a mobile React 
Native app as in the browser doesn't mean you should do this every time. You must be vigilant, 
scan your dependencies for usability and keep checking whether they're still necessary or could 
be replaced with better platform-specific alternatives.
Internationalization polyfills
The Intl global object provides language-sensitive string comparison, number formatting, 
and date and time formatting. It's available in any modern browser and partially in the Hermes 
engine.
Here is an example of formatting a number to a different locales using Intl:
const number = 123456.789;
const germanFormat = new Intl.NumberFormat('de-DE');
console.log(germanFormat.format(number)); // 123.456,789
In React Native projects, you can often spot a bunch of Intl polyfill imports from the Format.
JS project, providing JS-only implementation of ECMAScript Internationalization API:

import '@formatjs/intl-getcanonicallocales/polyfill';
import '@formatjs/intl-locale/polyfill';
import '@formatjs/intl-numberformat/polyfill';
import '@formatjs/intl-numberformat/locale-data/en';
import '@formatjs/intl-datetimeformat/polyfill';
import '@formatjs/intl-datetimeformat/locale-data/en';
import '@formatjs/intl-pluralrules/polyfill';
import '@formatjs/intl-pluralrules/locale-data/en';
import '@formatjs/intl-relativetimeformat/polyfill';
import '@formatjs/intl-relativetimeformat/locale-data/en';
import '@formatjs/intl-displaynames/polyfill';
Importing all possible Intl polyfills is not always necessary
The support for this API has been limited in Hermes, but fortunately, last year, the Hermes 
team announced they were working on improving it. A year later, some of those polyfills can 
be removed! At the time of writing this guide (January 2025), here are the available APIs:
Intl APIHermes support
Intl.Collator 
✅
Intl.DateTimeFormat
✅
Intl.NumberFormat
✅
Intl.getCanonicalLocales()
✅
Intl.supportedValuesOf()
✅
Intl.ListFormat
❌
Intl.DisplayNames
❌
Intl.Locale
❌
Intl.RelativeTimeFormat
❌
Intl.Segmenter
❌
Intl.PluralRules
❌
With that new knowledge, we can reduce the number of polyfills we import into our project:
-import '@formatjs/intl-getcanonicallocales/polyfill';
 import '@formatjs/intl-locale/polyfill';
-import '@formatjs/intl-numberformat/polyfill';
-import '@formatjs/intl-numberformat/locale-data/en';
-import '@formatjs/intl-datetimeformat/polyfill';
-import '@formatjs/intl-datetimeformat/locale-data/en';
 import '@formatjs/intl-pluralrules/polyfill';
 import '@formatjs/intl-pluralrules/locale-data/en';
 import '@formatjs/intl-relativetimeformat/polyfill';
 import '@formatjs/intl-relativetimeformat/locale-data/en';
 import '@formatjs/intl-displaynames/polyfill';
Importing all possible Intl polyfills is not always necessary

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

Native bottom tabs from react-native-bottom-tabs
Prefer using libraries offering native components
Libraries exposing native components should be your first choice. React Native has a wide 
community of developers working on open-source libraries that expose native components. 
Let's go over a few of those:
 ■React Native Screens—foundation for React Navigation's Native Stack.
 ■Zeego—beautiful, native menus for React Native + Web, inspired by Radix UI.
 ■React Native Slider—slider using native platform primitives.
 ■React Native Date Picker—date picker using native platform primitives.
And there are many more!
Replacing infinitely flexible JavaScript views with platform-native counterparts may not always 
be possible due to your app's design requirements. If a better, more performant UX alternative 
exists but requires you to adjust your design choices slightly, you may need to stand up for your 
users. Great UX is about the dialog between designers' creativity and what's physically possible 
from an engineering standpoint. Don't be afraid to voice your concerns. After all, we do it all 
for our users.
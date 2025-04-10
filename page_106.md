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
Best Practice: Use Dedicated React Native SDKs Over Web
The Ultimate Guide to React Native Optimization
117
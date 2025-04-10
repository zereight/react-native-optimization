BEST PRACTICE
HIGHER-ORDER SPECIALIZED 
COMPONENTS
In a React Native application, almost everything is a component. At the end of the component 
hierarchy are the so-called primitive components, such as Text, View, or TextInput. These 
components are implemented by React Native and provided by the platform you are targeting 
to support the most basic user interactions.
When building our application, we compose it out of smaller building blocks. To do so, we use 
primitive components. For example, to create a login screen, we would use a series of  TextInput 
components to register user details and a Pressable component to handle user interaction. 
This approach is valid from the very first component that we create within our application and 
holds through the final stage of its development.
In addition to the primitive components, the react-native package and third-party libraries 
come with various higher-order components designed and optimized to serve a particular 
purpose. Not using them can affect your application's performance, especially as you populate 
your state with real production data.
Displaying lists
Let's take lists as an example. Every application contains a list at some point. The default way 
we know from web development is to create a list of elements would be to combine  <View /> 
components inside a <ScrollView />:
import { View, Text, ScrollView } from 'react-native';
const NUMBER_OF_ITEMS = 10;
const App = () => {
  const items = Array.from({ length: NUMBER_OF_ITEMS }, (_, index) 
Best Practice: Higher-Order Specialized Components
The Ultimate Guide to React Native Optimization
34
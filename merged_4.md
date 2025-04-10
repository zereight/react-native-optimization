setValue(text);
  };
  return (
    <TextInput
      onChangeText={onChangeText}
-     value={value}
    />
  );
};
As a result, the data will flow only one way, from the native RCTSinglelineTextInputView 
on iOS or AndroidTextInputNativeComponent on Android to the JavaScript side. Native 
components emit onChangeText and control the internal input state on their own, eliminating 
the synchronization issues described earlier.

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

=> `Item ${index + 1}`);
  return (
      <ScrollView>
        {items.map((item, index) => (
          <View key={index} >
            <Text >{item}</Text>
          </View>
        ))}
      </ScrollView>
  );
};
export default App;
Source: https://snack.expo.dev/@callstack-snack/9374a7 
Such an example, while working well in the initia stages of application development, will 
quickly become problematic. Let's look at what happens when the query returns 5000 items 
for this list:
- const NUMBER_OF_ITEMS = 10;
+ const NUMBER_OF_ITEMS = 5000;
As you can see on the graph below, performance, measured by FPS, dropped significantly to 
the point where the application became unresponsive for around a second.

Switching to FlatList
The abovementioned issue can be quickly resolved by moving from a primitive solution to 
a specialized FlatList component. FlatList comes bundled with React Native and is designed 
to do one thing well: display a large number of elements in a list. Let’s replace our ScrollView 
with a FlatList and see how it affects performance:
-import { View, Text, ScrollView } from 'react-native';
+import { View, Text, FlatList } from 'react-native';
const NUMBER_OF_ITEMS = 5000;
const App = () => {
  const items = Array.from({ length: NUMBER_OF_ITEMS }, (_, index) 
=> `Item ${index + 1}`);
+ const renderItem = ({ item }) => (
+   <View>
+     <Text>{item}</Text>
+   </View>
+ );
  return (
-     <ScrollView>
-       {items.map((item, index) => (
-         <View key={index} >
-           <Text >{item}</Text>
-         </View>
-       ))}
-     </ScrollView>
+     <FlatList
+       data={items}
+       renderItem={renderItem}
+       keyExtractor={(item, index) => index.toString()}
+     />
  );
};
export default App;
Source: https://snack.expo.dev/@callstack-snack/c17e89 

A graph illustrating the FPS performance when initiating list using FlatList component
The difference is quite stark. You might be wondering what causes such a significant performance 
improvement. At the end of the day, <FlatList /> uses the same primitive components under 
the hood and eventually displays View s inside a ScrollView.
How FlatList is faster than a ScrollView
The key lies in the logic abstracted away within <FlatList /> component. It contains a lot of 
heuristics and advanced JavaScript calculations to reduce the number of extraneous renderings 
that happen while you're displaying the data on screen and to make the scrolling experience 
always run at 60 FPS.
FlatList internally uses a VirtualizedList, which implements "windowin"—a techn'que 
that only renders and mounts items currently visible in the viewport plus a small buffer. 
As you scroll, it dynamically unmounts items that move out of view and mounts new ones 
coming into view, maintaining a finite render window of active items. This significantly 
reduces memory usage and ensures smooth scrolling performance in most scenarios.
Rendering complex elements in a FlatList
However, using FlatList alone may not be enough in some cases. Its performance 
optimizations rely on not rendering elements that are currently off-screen. 
That said, the most costly part of the entire process are layout measurements. 
FlatList must 
measure your layout to determine how much space in the scroll area should be reserved for 
upcoming elements. For complex list elements, this may slow down the interaction, as the 
component will have to wait for all the items to render to measure the layout.
The example now runs without dropping frames as we scroll. 

To mitigate this, you can implement getItemLayout() to define the element height up front, 
eliminating the need for measurement. 
import { View, Text, FlatList } from 'react-native';
const NUMBER_OF_ITEMS = 10;
+ const ITEM_HEIGHT = 50; // Define a height of a list item 
component or calculate it
const App = () => {
  const items = Array.from({ length: NUMBER_OF_ITEMS }, (_, index) 
=> `Item ${index + 1}`);
  const renderItem = ({ item }) => (
-   <View>
+   <View style={{ height: ITEM_HEIGHT }}>
      <Text>{item}</Text>
    </View>
  );
+ const getItemLayout = (_, index) => ({
+   length: ITEM_HEIGHT,
+   offset: ITEM_HEIGHT * index,
+   index,
+ });
  return (
    <FlatList
      data={items}
      renderItem={renderItem}
      keyExtractor={(item, index) => index.toString()}
+     getItemLayout={getItemLayout}
    />
  );
};
export default App;
Source: https://snack.expo.dev/@callstack-snack/2f7253
It is not straightforward for items without a constant height. However, you can calculate the 
value based on the number of lines of text and other layout constraints.
FlashList as an alternative to FlatList
As already discussed, FlatList drastically improves the performance of a huge list compared 
to ScrollView. Despite being a performant solution, it has some caveats, such as rendering 
blank spaces while scrolling, laggy scrolling, and a list not being snappy. Also, FlatList was 

designed to keep certain elements in memory, which adds overhead to the device and eventually 
slows the list down.
Following the tips in the official documentation can minimize the frequency of the 
aforementioned symptoms to some extent. Still, in most cases, we want smoother 
and snappier lists without extra work.
With FlatList, the JS thread is busy most of the time, and we always fancy having that 60 FPS 
tag associated with it when we're scrolling the list.
So, how should we approach such issues? Luckily, Shopify developed a pretty good drop-in 
replacement, 
FlashList. 
import React from 'react';
import { View, Text } from 'react-native';
import { FlashList } from "@shopify/flash-list";
const NUMBER_OF_ITEMS = 10;
const ITEM_HEIGHT = 50;
const App = () => {
  const items = Array.from({ length: NUMBER_OF_ITEMS }, (_, index) 
=> `Item ${index + 1}`);
  const renderItem = ({ item }) => (
    <View style={{ height: ITEM_HEIGHT }}>
      <Text>{item}</Text>
    </View>
  );
  return (
    <FlashList
      data={items}
      renderItem={renderItem}
      estimatedItemSize={ITEM_HEIGHT}
    />
  );
};
export default App;
The library works on top of RecyclerListView, leveraging its recycling capability and 
fixing common pain points such as complicated API, using cells with dynamic heights, 
or first render layout inconsistencies.
FlashList recycles views outside of the viewport and reuses them for other items. If the list has 
different items, it uses a recycle pool to use the item based on its type. It's crucial to keep the list 

An example of differed sized-items
If your list contains items of different sizes, you can calculate the average size of the items (50px, 
100px, and 150px) to get the estimated item size (50px + 100px + 150px) / 3 = 100px.
If you do not supply this value, it will be provided by the list as a warning on the 
first render. It is recommended not to ignore that warning and define this prop 
explicitly before the list gets to your users.
How much faster is it? 
items as light as possible, without any side effects; otherwise, it will hurt the list's performance.
Estimated item size
Aside from the data and renderItem props that you already know from working with 
FlatList, there's one additional and quite important prop: estimatedItemSize. It is 
the approximate size of the list item that is used by FlashList to decide how many items 
to render before the initial load and while scrolling.
A report comparing performance when running FlatList and FlashList examples

As you can see from the report, FlashList significantly outperforms FlatList with a performance 
score of 68/100 vs. 25/100, demonstrating superior efficiency across all metrics, including faster 
runtime and lower resource consumption. The frame rate graph further reinforces FlashList's 
dominance, showing more stable performance at ~56 FPS. It looks like there's still some room 
for improvement.
You  can  also  leverage FlashList  FlashList's  callback  functions  to  measure 
performance and rendering times. The Metrics  section  of  the  documentation  
provides more information about available helpers.
Keep an eye on Legend List
As of early 2025, Jay Meistrich has intensively developed a new alternative to FlatList called 
Legend List.  It is currently versioned as 1.0-beta and presents a very similar performance to 
FlashList. It leverages New Architecture possibilities and is implemented fully in JavaScript. 
While not production-ready yet, it's definitely something that the React Native community 
should keep an eye on.
A comparison of LegendList and FlashList performance
Using specialized components may not always yield the fastest results possible, but as shown 
by the examples of FlatList, FlashList, and LegendList, they're easily replaceable, giving you 
more options to pick from and validate. And as with any other third-party dependencies, with 
newer versions, you can get more optimized code for free—or not! Always measure.
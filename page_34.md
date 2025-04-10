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
Best Practice: Higher-Order Specialized Components
The Ultimate Guide to React Native Optimization
36
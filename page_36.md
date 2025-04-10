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
Best Practice: Higher-Order Specialized Components
The Ultimate Guide to React Native Optimization
38
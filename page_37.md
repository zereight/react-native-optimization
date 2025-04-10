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
Best Practice: Higher-Order Specialized Components
The Ultimate Guide to React Native Optimization
39
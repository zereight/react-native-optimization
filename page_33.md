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
Best Practice: Higher-Order Specialized Components
The Ultimate Guide to React Native Optimization
35
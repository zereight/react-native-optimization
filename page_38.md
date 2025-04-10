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
Best Practice: Higher-Order Specialized Components
The Ultimate Guide to React Native Optimization
40
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
Best Practice: Higher-Order Specialized Components
The Ultimate Guide to React Native Optimization
37
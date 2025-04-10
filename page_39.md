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
Best Practice: Higher-Order Specialized Components
The Ultimate Guide to React Native Optimization
41
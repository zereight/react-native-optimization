How to verify optimizations?
Open React DevTools to see which components have been optimized by the React Compiler. 
Optimized components are labeled with a Memo   badge, which shows that the compiler has 
applied optimizations.
A single update in SearchPageBottomTab causes 
cascading re-renders across deeply nested 
components, increasing rendering time
React Compiler blocks unnecessary updates, reducing 
the re-render scope and ensuring that only truly 
affected components update
Best Practice: React Compiler
The Ultimate Guide to React Native Optimization
57
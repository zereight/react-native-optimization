GUIDE
HOW TO PROFILE JS 
AND REACT CODE
When optimizing performance, we want to know exactly which code path is the source of 
a slowdown. With some experience and knowledge of the codebase and common performance 
pitfalls, developers can often identify the issues intuitively and expect the app to run faster. In 
reality, though, apps are usually too complex for anyone to know the whole system by heart. 
Typical blind optimizations we make have little impact on the overall performance. To be 
precise with our actions, we must make decisions guided by data. 
The data comes from measuring performance using specialized tools. This process is often 
referred to as "profiling", as it involves analyzing a program to create a "profile" of its execution. 
This profile typically includes information about which parts of the program consume the 
most resources, such as CPU time or memory, helping identify performance bottlenecks.
Profiling with React Native DevTools
In the context of a React Native app, it's helpful to be able to profile React itself to inspect 
unnecessary re-renders. The best tool to profile React running in a mobile app is React Native 
DevTools, and that's what we'll focus on in this guide. 
React Profiler is integrated as the default plugin in React Native DevTools and can produce 
a flame graph of the React rendering pipeline as a result of profiling. We can leverage this data 
to analyze the app's re-rendering issues. 
Here is the code we're about to profile:
export const App = () => {
  const [count, setCount] = React.useState(0);
  const [second, setSecond] = React.useState(0);
  return (
    <View style={styles.container}>
Guide: How to Profile JS and React Code
The Ultimate Guide to React Native Optimization
14
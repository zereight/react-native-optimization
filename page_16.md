// wrap inline functions with `useCallback`
const onPressHandler = useCallback(() => setCount(count + 1), 
[count]);
const secondHandler = useCallback(() => setSecond(second + 1), 
[second]);
// ...
<Button onPress={onPressHandler} title="Press one" />
<Button onPress={secondHandler} title="Press two" />
// wrap Button component with `memo`
const Button = memo(({onPress, title}) => {
  // ...
});
After wrapping Button with React.memo and its onPress handlers with React.useCallback, 
we get a different picture from the profiler, showing the same number of 4 React commits. 
However, now, only the button that's actually pressed is re-rendered, while the other one 
gets memoized (with auto-generated _c2 name), which is indicated by the green color of this 
component and its children. 
A flame graph of the same flow but no with one of the buttons not being updated due to memoization 
Now, you're well-equipped to start your adventure with profiling any React application—
whether it's a web app or mobile app. We strongly advise you to take a break from reading now 
and try it in the apps you develop on a daily basis. You'll notice the flame graph output will be 
much noisier due to the bigger complexity of your React app. 
Remember to focus on the components marked with yellow as the ones where 
React spends the most time. And leverage the "Why did this render?" information. 
Profiling JavaScript Code
React Native DevTools provide great utilities for profiling not only React but also JavaScript 
code. After all, it's based on Chrome DevTools, reusing its backend. In this section, we will 
Guide: How to Profile JS and React Code
The Ultimate Guide to React Native Optimization
18
React Native Reanimated is an industry standard for making animations. As of 
January 2025, it's a battle-tested solution with almost 1 million weekly downloads.
With Reanimated, you can easily run animations directly on the UI thread, thanks to the 
concept of worklets. Worklets are short-running JavaScript functions that can be run on the 
UI thread. They can also be run on a JavaScript thread just as you would run a function in your 
code.
Pioneered by Reanimated, worklets are a powerful concept that has found its way 
into other community libraries, such as VisionCamera or LiveMarkdown. Such 
libraries can use the 
createWorkletRuntime API to create a new JS runtime, 
which can be used to run worklets on different threads than the JS or UI thread.
Most of the time, when working with Reanimated, the code runs on the UI thread by default, 
such as in the case of the useAnimatedStyle hook:
const style = useAnimatedStyle(() => {
  console.log('Running on the UI thread');
  return { opacity: 0.2 };
});
Reanimated comes with a variety of helpers. One that we'd like to pay special attention to is 
runOnUI. It allows you to run any code on the UI thread manually. It can be used, for example, 
with  an  useEffect to start an animation on component mount/unmount or with measure 
and scrollTo functions, which have implementations only on the UI thread. You can use it 
like this:
runOnUI(() => {
  console.log('Running on the UI thread');
});
There's also another utility function caled runOnJS, which you can leverage to run functions 
that are not worklets and couldn't run on UI thread, e.g., functions that come from external 
libraries.
// Function that needs to run on JS thread
const notifyCompletion = () => {
  console.log('Animation completed!');
  // Could be analytics tracking, state updates, etc.
};
Best Practice: High-Performance Animations Without Dropping Frames
The Ultimate Guide to React Native Optimization
60
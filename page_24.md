Now that you know what typical memory leaks look like, let's see what tools you can use 
to spot them easily.
Hunting memory leaks with React Native DevTools
The new React Native DevTools builds on top of Chrome DevTools, so if you are familiar 
with web debugging, you should feel right at home. However, if you want to learn more about 
Chrome DevTools, you should check out their official documentation.
React Native DevTools offers three ways to profile your app's memory: 
 ■Heap snapshot—shows memory distribution among your page's JavaScript objects 
and related DOM objects.
 ■Allocation instrumentation on the timeline—this profile type shows instrumented 
JavaScript memory allocations over time. Once a profile is recorded, you can select a 
time interval to see objects that were allocated within it and still live by the end of the 
recording. Use this profile type to isolate memory leaks.
 ■Allocation sampling—records memory allocations using the sampling method. This 
profile type  has minimal performance overhead and can be used for long-running 
operations. It provides good approximations of allocations broken down by the 
JavaScript execution stack.
We'll focus on Allocation  instrumentation  on  the  timeline,  which should help us find 
allocations that are not being freed up. Let's run it on this piece of code: 
// Global variable to store closures
let leakyClosures: Funciton[] = [];
// Generate some dummy data
const generateLargeData = () => {
  return new Array(1000).fill(null).map(() => ({
    id: Math.random().toString(),
    data: new Array(500).fill('??').join(''),
    timestamp: new Date().toISOString(),
    nested: {
      moreData: new Array(100).fill({
        value: Math.random(),
        text: 'nested data that consumes memory',
      }),
    },
  }));
};
const createClosure = () => {
  const newDataLeak = generateLargeData();  // Creates large data 
array
  return () => {
Guide: How to Hunt JS Memory Leaks
The Ultimate Guide to React Native Optimization
26
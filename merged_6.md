target: '19' // <- pick your 'react' version
};
module.exports = function () {
  return {
    plugins: [
      ['babel-plugin-react-compiler', ReactCompilerConfig],
    ],
  };
};
babel.config.js 
If you want to use React Compiler on a project that's lower than React Native 0.78 (which 
ships with React 19), you'll need to add one extra package: react-compiler-runtime@beta 
and configure the target to "18" accordingly in ReactCompilerConfig. Now you're all set! 
Or almost set.
You will likely notice that compiler may fail on certain files. For such occasions, the Babel 
plugin allows for some level of customization with sources config, where you can skip some 
components or even whole directories:
const ReactCompilerConfig = {
  sources: (filename) => {
    return filename.indexOf('src/path/to/dir') !== -1;
  },
};
babel.config.js
This way you, can add React Compiler incrementally into your project, lowering the risk of 
regressions caused by bugs in beta software. Let's see how the compiler affects our source code 
with a short example of a TextInput with onChangeText callback:
export default function MyApp() {
  const [value, setValue] = useState('');
  return (
    <TextInput
      onChangeText={() => {
        setValue(value);
      }}>
      Hello World
    </TextInput>
  );
}
Source code before running compiler

import { c as _c } from 'react/compiler-runtime';
export default function MyApp() {
  const $ = _c(2);
  const [value, setValue] = useState('');
  let t0;
  if ($[0] !== value) {
    t0 = <TextInput onChangeText={() => setValue(value)}>Hello 
World</TextInput>;
    $[0] = value;
    $[1] = t0;
  } else {
    t0 = $[1];
  }
  return t0;
}
After running compiler: more code, less re-renders
Notice how React Compiler transformed our code. It imports a c function from the react/
compiler-runtime and uses it to determine what to render based on state. The c(n) function is 
a polyfill of useMemoCache(n) from React 19, allowing the same behavior in earlier versions of 
React. It creates an array that persists across renders, similar to useRef. If we were to implement 
it using useRef, it would look like this:
function useMemoCache(n) {
  const ref = useRef(Array(n).fill(undefined));
  return ref.current;
}
useMemoCache creates an array with n slots that persist across renders
In the example above, const  $  =  _c(2) works like useMemoCache(2), returning an array 
with two slots. $[0] holds the previous value, while $[1] stores the last rendered TextInput. 
When 
value changes, a new TextInput is stored in $[1]. If value remains the same, React 
reuses the cached component instead of re-rendering it.
React Compiler uses shallow comparison, just like React.memo and useMemo, so 
be careful when using objects or arrays as props. If their reference changes, they 
will be treated as new values.
React Compiler Playground
If you want to understand how the React Compiler transforms your code, the React Compiler 
Playground is your friend. It allows you to inspect how the compiler optimizes components, test 
different structures, and debug compiler output. This interactive environment helps you see 
what changes the compiler makes and identify any unexpected behaviors in your components.

Is it time to remove manual memoization?
Not yet. While the React Compiler automates memoization, it does not fully replace 
React.memo, useMemo, or useCallback in all cases. If your code heavily relies on these 
optimizations, you should keep them in place until the compiler reaches a stable release 
and official recommendations from the React team confirm that manual memoization is no 
longer needed. We expect the linter to guide you on which optimizations are unnecessary.
What performance improvements can you expect?
React Compiler aims to reduce unnecessary re-renders and minimize the need for manual 
optimizations. By automatically memoizing component calculations, it prevents cascading 
updates and improves performance, particularly in large applications with deep component 
trees. 
Testing the React Compiler on the Expensify app showed measurable improvements in 
performance. For example, one of its most important metrics, "Chat Finder Page Time to 
Interactive" (effectively TTI) improved by 4.3%. While the React Compiler significantly reduces 
unnecessary re-renders, applications already optimized with manual memoization techniques 
may see only marginal improvements.

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

React  Compiler  is  built  for  universal  React  but  primarily  tested  in  web 
environments. Enabling it in React Native might require additional steps to ensure 
Memo  badges appear correctly.
Since React Native includes its own version of  react-devtools, you may need 
to override it in your package.json and ensure it's updated to version 6.0.1 or 
newer. Without this, Memo  badges may not appear.
To check your React DevTools backend version, open DevTools and click the  
settings icon.

BEST PRACTICE
HIGH-PERFORMANCE 
ANIMATIONS WITHOUT 
DROPPING FRAMES
Animations play a crucial role in creating engaging mobile experiences. In React Native, 
ensuring smooth animations that run at least at 60 frames per second (FPS) is essential but 
sometimes challenging, especially when handling high-load operations. When animations drop 
frames or appear janky, they can significantly impact the user experience and make your app 
feel unpolished. In this chapter, we'll explain which tools you can use to ensure your animations 
are running as expected, their limitations, and best practices to follow.
Understanding the main thread in React Native
First, let's understand what the main thread is. In React Native, the main thread (also called the 
UI thread) is the primary thread responsible for handling UI rendering and user interactions. 
It's the thread where all visual updates happen and where the user interface responds to touch 
events.
In React Native, the terms "main thread" and "UI thread" are used interchangeably. 
When developers talk about either one, they refer to the same thing.
This is different from the JavaScript thread, where most of your React Native code runs, which 
we described more broadly in the introduction.
React Native Reanimated
When you want to add animations to your React Native app, you should go with React Native 
Reanimated, a powerful library that provides first-class support for performant animations.

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

const animatedStyle = useAnimatedStyle(() => {
  if (progress.value >= 1) {
    // Call JS thread function from UI thread
    runOnJS(notifyCompletion)();
  }
  return {
    opacity: progress.value,
    transform: [{ scale: withSpring(progress.value) }],
  };
});
The choice of which thread to run your code depends on several factors:
UI thread (main thread)
 ■Best for visual animations, transforms, and layout changes.
 ■Use when you need direct manipulation of UI elements.
JavaScript thread
 ■Best for complex calculations, data processing, and state management.
 ■Use when you need to access React state or props or perform business logic.
InteractionManager
React Native comes with InteractionManager API, which allows long-running work to 
be scheduled after any interactions/animations have been completed. This allows JavaScript 
animations to run smoothly, helping avoid jank and dropped frames.
When users interact with the app (e.g., tapping a button or handling gestures), executing heavy 
tasks on the JavaScript thread simultaneously with UI updates can lead to performance issues 
and undesirable behavior. With InteractionManager, we can schedule non-critical tasks to 
run after all current interactions have been completed:
InteractionManager.runAfterInteractions(() => {
  console.log('Running after interactions');
});
The touch handling system considers one or more active touches to be an 'interaction' and will 
delay runAfterInteractions() callbacks until all touches have ended or been canceled.
You can also register animations with InteractionManager.createInteractionHandle 
function, which, when cleared manually after interaction, will run all the scheduled actions.

const handle = InteractionManager.createInteractionHandle();
// run animation... (`runAfterInteractions` tasks are queued)
// later, on animation completion:
InteractionManager.clearInteractionHandle(handle);
// queued tasks run if all handles were cleared
Pairing it with React Navigation
A typical scenario where you'd like to use InteractionManager is with React Navigation's 
useFocusEffect. This hook runs the effect as soon as the screen comes into focus. It often 
means that if there is an animation for the screen change, it might not have finished yet.
React Navigation runs its animations on the UI thread, so it's not a problem in many cases. 
However, if the effect updates the UI or renders something expensive, it can affect the animation 
performance. In such cases, we can use InteractionManager to defer our work until the 
animations or gestures have finished:
useFocusEffect(
  React.useCallback(() => {
    const task = InteractionManager.runAfterInteractions(() => {
      // Expensive task eventually updating UI
    });
    return () => task.cancel();
  }, [])
);
Alternatively to InteractionManager, which is a React Native-specific API, you can also 
leverage the latest additions to React, such as the startTransition hook, to move non-
critical updates for later execution to not block the UI update. Read more about it in the Using 
transitions for non-critical updates chapter.
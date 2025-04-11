# React Native Optimization
## The Ultimate Guide

## TABLE OF CONTENTS

### Introduction
- [Preface](#preface)
- [How to Read This Book](#how-to-read-this-book)
- [Why Performance Matters](#why-performance-matters)

### Part 1: JavaScript
- [How to Profile JS and React Code](#how-to-profile-js-and-react-code)
- [How to Measure JS FPS](#how-to-measure-js-fps)
- [How to Hunt JS Memory Leaks](#how-to-hunt-js-memory-leaks)
- [Uncontrolled Components](#uncontrolled-components)
- [Higher-Order Specialized Components](#higher-order-specialized-components)
- [Atomic State Management](#atomic-state-management)
- [Concurrent React](#concurrent-react)
- [React Compiler](#react-compiler)
- [High-Performance Animations Without Dropping Frames](#high-performance-animations-without-dropping-frames)

### Part 2: Native
- [Understand Platform Differences](#understand-platform-differences)
- [How to Profile Native Parts of React Native](#how-to-profile-native-parts-of-react-native)
- [How to Measure TTI](#how-to-measure-tti)
- [Understanding Native Memory Management](#understanding-native-memory-management)
- [Understand the Threading Model of Turbo Modules and Fabric](#understand-the-threading-model-of-turbo-modules-and-fabric)
- [Use View Flattening](#use-view-flattening)
- [Use dedicated React Native SDKs Over Web](#use-dedicated-react-native-sdks-over-web)
- [Make Your Native Modules Faster](#make-your-native-modules-faster)
- [How to Hunt Memory Leaks](#how-to-hunt-memory-leaks)

### Part 3: Bundling
- [How to Analyze JS Bundle Size](#how-to-analyze-js-bundle-size)
- [How to Analyze App Bundle Size](#how-to-analyze-app-bundle-size)
- [Determine True Size of Third-Party Libraries](#determine-true-size-of-third-party-libraries)
- [Avoid Barrel Exports](#avoid-barrel-exports)
- [Experiment With Tree Shaking](#experiment-with-tree-shaking)
- [Load Code Remotely When Needed](#load-code-remotely-when-needed)
- [Shrink Code With R8 Android](#shrink-code-with-r8-android)
- [Use Native Assets Folder](#use-native-assets-folder)
- [Disable JS Bundle Compression](#disable-js-bundle-compression)
- [Reduce Bundle Size](#reduce-bundle-size)

### Additional Information
- [Acknowledgements](#acknowledgements)
- [About The Authors](#about-the-authors)
- [Libraries and Tools Mentioned in This Guide](#libraries-and-tools-mentioned-in-this-guide)

## PREFACE

React Native has come a long way. From the early days as a promising experiment in cross-platform development to adoption by some of the world's largest enterprises, its evolution has been nothing short of remarkable. The framework has matured, proving capable of handling the demands of high-scale applications while continuing to refine and push its technical boundaries.

For years, we have seen React Native introduce new features, often groundbreaking, with every release. Today, we see steady, low-level improvements that often enhance performance, stability, and scalability. These refinements reflect how React Native is being adopted more widely, reinforcing its role as a mature and enterprise-ready framework.

A significant milestone in this journey was the introduction of the New Architecture, which redefined many core aspects of the framework, including how we think about development in general and performance optimizations. It brought new features and best practices, enabling developers to build even more efficient and scalable applications.

And so, we decided it was time to overhaul our guide to optimization completely. The landscape has changed, and so must our approach. Some techniques that were once essential are no longer relevant; others have taken on new importance. In this new edition, we aim to equip developers with the latest insights, tools, and strategies to make the most of React Native's evolving capabilities.

Whether you're an experienced React Native engineer or just getting started, this guide goes beyond performance best practices. It also provides essential knowledge to help you understand React Native in general, including what happens under the hood. We rely on that knowledge every day to build better, more efficient, and scalable applications, and we hope it will help you do the same.

— Mike Grabowski, Callstack's Founder & CTO

## HOW TO READ THIS BOOK

This book is intended for React Native developers of various levels of experience. We believe that both newcomers and seasoned React Native devs, regardless of whether they come from a web or native app development background, will find something valuable and applicable in their apps.

We acknowledge that at a given time you may not be interested in all the optimizations presented in this book. Oftentimes, your focus will be on a specific area, such as optimizing React re-renders. That's why, although you can read the whole book linearly, we made sure it's easy to open on any given chapter and get right to the topic of your interest.

### What's waiting for you inside?

To make it easier for everyone to find relevant information, we split this guide into three parts focusing on different kinds of optimization you may be particularly interested in: the React side, the Native side, and overall build-time optimizations. All three parts contain an introduction to the main topic, detailed guides, and best practices to help you improve the most important metrics describing your app's performance: FPS (Frames Per Second) and TTI (Time to Interactive).

Here's what you can expect in each part:
- **Introduction** — where we present key topics, terms, and ideas related to the main topic.
- **Guides** — where we explain how to use specialized tools and measure important metrics.
- **Best practices** — where we show you what to do to make your app run and initialize faster.

### Conventions used in this book

To better illustrate the ideas presented in this book, we've included a lot of short code snippets, screenshots from the tools we use, and diagrams.

We link to external resources, such as official docs or libraries, but also to other chapters of this book.

To give you extra context on certain topics, we decided to put them in callouts—while not intended for linear reading, we highly recommend not skipping those when reading.

### About Callstack

At Callstack, we are committed to advancing React Native and empowering developers to build high-performance applications. As core contributors and Meta partners, we work closely with the community—shaping proposals, maintaining key modules, and driving the framework's evolution. Our team actively contributes to React Native's monthly releases, ensuring developers have access to the latest improvements. By sharing our expertise, tools, and best practices, we help teams solve performance challenges, optimize their apps, and push the boundaries of what's possible with React Native.

### Let's get in touch

React Native thrives because of its community, and you can be part of its evolution. Whether by optimizing your own apps, contributing to open-source projects, or staying informed about the latest advancements, your involvement helps push the ecosystem forward.

Here's how you can stay engaged:
- **Stay up to date** — subscribe to our newsletter and follow our latest insights, open-source contributions, and best practices.
- **Join the conversation** — Join our Discord server to discuss proposals, share your ideas, and help shape the future of React Native.
- **Contribute** — explore and collaborate on our open-source projects.

And if you want to get in touch, contact us anytime. Let's build a faster, better React Native together!

## WHY PERFORMANCE MATTERS

When building mobile apps, performance is not just a technical concern—it's a user experience priority. A fast, responsive app can make the difference between a delighted user and one who abandons your app entirely. But what does "fast" really mean? Is it about raw numbers or maybe a user's perception?

### The user's perspective

Perceived performance is all about how fast your app feels to the user. It's not just about raw numbers or benchmarks—it's about creating the illusion of speed.

In the 1940s, an office building in New York faced complaints about slow elevators. To address this, instead of speeding up the elevators, the building manager installed mirrors near the elevators. This simple change distracted people by giving them something to do (look at themselves), reducing perceived wait times, and effectively solving the issue. While this story may be more of an urban legend, the concept itself is rooted in valid psychological principles about perceived time and distraction that influence the feeling of how fast a particular event is.

When a mobile app takes a few seconds to load, showing a splash screen, skeleton UI, or even a game to play can make users feel like the app is responsive and ready to use. That's why perceived performance is often more important than actual performance in shaping user satisfaction.

However, focusing solely on perceived performance can be misleading. While tricks like animations, placeholders, or preloading content can improve the user's perception, they don't address the underlying performance issues. That's where measurable metrics like TTI and FPS show their value.

### The metrics that matter: TTI and FPS

Out of the many metrics you can measure and monitor, two have the most impact on how users perceive the speed of your app. It's how quickly they can interact with the app described by TTI and how fluid the app feels when interacting with it described by FPS at a given time.

#### Time to Interactive (TTI)

Measuring the app's boot-time performance is described by the TTI metric. It measures how quickly your app becomes usable after launch. This is the moment when users can start interacting with the app without delays or jank. But TTI isn't just about the app's initial load. Companies often track variations of this metric, such as Time to Home (how long it takes to load the home screen) or Time to Specific Screen (how long it takes to navigate to a key feature). These metrics are especially important for apps with complex navigation flows or heavy initial data fetching.

If your app takes too long to load, users may abandon it before they even get a chance to explore its features or simply get fed up with it being slow anytime they try to do something, making them feel miserable and starting to associate your app, and sometimes even its brand, with that feeling.

#### Frames Per Second (FPS)

Once your app is up and running, FPS becomes the key metric for runtime performance. FPS measures how smoothly your app responds to user interactions, such as scrolling, swiping, or tapping buttons. A high FPS (ideally 60 frames per second or more) ensures that animations and transitions feel smooth and natural. When FPS drops, user experience lags, and animations begin to stutter, making your app feel unpolished or slow to respond. This is especially critical for apps with rich visual content or complex interactions.

### What does fast even mean?

As much as we love data and a scientific approach to validating our hypotheses, we realize that there is insufficient amount of independent and high-quality research on what "fast" means for mobile users. Usually, it's in the form of big tech companies boasting about their conversion numbers or paid reports based on surveys.

Regardless, what's even more important is that the feeling of speed is a moving target. With access to higher-end and better-performing devices, while using plenty of different apps on a single smartphone, users' expectations are growing higher. It's also worth remembering that these expectations are vastly dependent on the demographic—e.g., younger generations will typically expect apps to load and operate faster than their parents and grandparents, who are used to things taking more time. The expectations and incentives to improve are also different for internal enterprise app users compared to individual customers.

What will typically give you the most reliable indication of whether your app is fast enough is knowing your users and your competition. The analytics you gather on your users' behavior will give you personalized insights into places where they tend to drop. Analyzing whether your competitors' apps are objectively faster or slower, on the other hand, will hint at whether performance is potentially the reason you're losing users.

In the first two parts of this book, we'll focus on how you can improve the runtime performance through JavaScript, React, and native optimizations. We will guide you through all the necessary tools and techniques you can utilize to better understand what influences FPS so you can fix it anywhere.

In the third part, we'll focus on the bundling processes of your app, which happen both on the JavaScript and native sides and are highly correlated with the boot-time performance. Similar to the first two parts of the guide, you'll be presented with everything you need to have a better overview of what's causing your app to load slowly and how to make it fast.

# PART 1: JAVASCRIPT
## Guides and techniques to improve FPS by optimizing JavaScript and React side of React Native

## Introduction

In the first part of this guide, we'll focus on the JavaScript part of the React Native ecosystem. As you probably already realize, React Native leverages the native platform primitives and glues together various technologies: Kotlin or Java for Android, Swift or Objective-C for iOS, C++ for React Native's core runtime, and JavaScript executed by the Hermes engine. It's a lot to take in, so let's break it down a bit and focus on one platform for simplicity: Android.

When the user opens a native Android app, it typically goes through a similar initialization path: the app opens on the main thread, it loads the native code—usually written in Kotlin, Java, or C++—into memory, and then executes this code, which usually results in some kind of UI being shown to the user. It's not so different for an Android app created with React Native. After all, it's a native app but with a twist: part of the native code being executed comes from React Native.

### JavaScript initialization path

![A simplified threading model of React Native in New Architecture presenting context in which JS is executed](https://example.com/image.png)

React Native initialization code handles a handful of key mechanisms:
- Initialization of React Native internals—cross-platform C++ React renderer, JavaScript Interface (JSI), Hermes JS engine, layout engine (Yoga)—on the main thread.
- Initialization of the JavaScript thread that runs JS and communicates back and forth with the main thread through the JSI.
- Initialization of the Native modules thread that runs lazily loaded Turbo Modules.

Notice how rendering logic (Kotlin, C++) and the so-called "business logic" are, by design, run on separate threads—the main thread vs. the JS thread. Since this part is about JavaScript and React, we'll focus on the JavaScript side that enables React to run on a dedicated JS thread and how this model affects the performance of your React Native apps.

According to an internal survey of 100 React Native developers at Callstack, 80% of the performance challenges they face in mobile, TV, and desktop React Native apps originate from the JavaScript side. This sample may not be representative of the real world due to its small size. However, seeing what the React Native Community is struggling with, it's pretty safe to assume that most of the performance issues are related to JavaScript. As such, it's wise to focus on these issues before moving to more advanced native optimizations described in Part 2: Native.

### React re-rendering model

React takes care of rendering and updating the application UI based on its state for you, regardless of the environment (or platform): web, iOS, Android, or anything else. The react library itself is tiny and consists mostly of public API definitions, some cross-platform functionality, and a reconciliation algorithm, which is responsible for efficiently updating the UI to reflect component state changes, effectively describing "what to do".

What makes this model powerful, though, is splitting the "what" from the "how" and deferring the latter to specialized renderers that manage how these updates are applied to different environments: react-dom for the web, react-native for iOS and Android, or react-native-windows for Windows, to name a few. This model allows you to define components of your app universally for all supported platforms simultaneously, and compose the final interface out of these smaller building blocks. With such an approach, you don't control the application rendering lifecycle; React and its renderer do the job for you.

![Visual description of React reconciliation algorithm](https://example.com/image.png)

In other words, deciding when to repaint things on screen is purely React's responsibility, while deciding about the "how" is the renderer's responsibility. React looks out for the changes you have done to your components, compares them, and, by design, only performs the required and the smallest number of actual updates.

React component re-renders in the following cases:
- Parent component re-renders
- State (including hooks) changes
- Props change
- Context changes
- Force update (escape hatch)

Before we move on to the practices that may or may not be relevant to your app's specific use case, let's start with something that you will find useful in every app you're going to build: profiling and measuring.

## GUIDE: HOW TO PROFILE JS AND REACT CODE

When optimizing performance, we want to know exactly which code path is the source of a slowdown. With some experience and knowledge of the codebase and common performance pitfalls, developers can often identify the issues intuitively and expect the app to run faster. In reality, though, apps are usually too complex for anyone to know the whole system by heart. Typical blind optimizations we make have little impact on the overall performance. To be precise with our actions, we must make decisions guided by data.

The data comes from measuring performance using specialized tools. This process is often referred to as "profiling", as it involves analyzing a program to create a "profile" of its execution. This profile typically includes information about which parts of the program consume the most resources, such as CPU time or memory, helping identify performance bottlenecks.

### Profiling with React Native DevTools

In the context of a React Native app, it's helpful to be able to profile React itself to inspect unnecessary re-renders. The best tool to profile React running in a mobile app is React Native DevTools, and that's what we'll focus on in this guide.

React Profiler is integrated as the default plugin in React Native DevTools and can produce a flame graph of the React rendering pipeline as a result of profiling. We can leverage this data to analyze the app's re-rendering issues.

Here is the code we're about to profile:

```jsx
export const App = () => {
  const [count, setCount] = React.useState(0);
  const [second, setSecond] = React.useState(0);
  return (
    <View style={styles.container}>
      <Text>Welcome!</Text>
      <Text>{count}</Text>
      <Text>{second}</Text>
      <Button onPress={() => setCount(count + 1)} title="Press one" />
      <Button onPress={() => setSecond(second + 1)} title="Press two" />
    </View>
  );
};

const Button = ({onPress, title}) => {
  return (
    <Pressable style={styles.button} onPress={onPress}>
      <Text>{title}</Text>
    </Pressable>
  );
};
```

When rendered, the `<App />` component will display a welcome message with two counters and buttons controlled by two separate state handlers. We're now ready to profile this React code with React Native DevTools.

You can access it by either pressing j in the Metro dev server or from the app using the React Native Dev Menu—accessed through a shake gesture on a device, Cmd+M on Android or Cmd+D on iOS—and pressing "Open DevTools". It will present you with a welcome screen, pointing to learning resources on the basics of debugging, DevTools features, and native debugging. We highly recommend getting familiar with these resources, some overlapping with the materials presented in this guide.

![React Native DevTools welcome screen](https://example.com/image.png)

Now, open the React Native DevTools. Go to the Profiler tab on top. Press the "gear" button to configure it to "Highlight updates when components render"

![General profiler settings accessed from "View Settings" icon in the Profiler tab of React Native DevTools](https://example.com/image.png)

and "Record why each component rendered while profiling".

![Profiler settings accessed from "View Settings" icon in the Profiler tab of React Native DevTool](https://example.com/image.png)

Finally, press the "Start profiling" or "Reload and start profiling" button on the top left of the screen to start profiling. It doesn't matter in our use case since we're inspecting reactions to user input. However, when profiling an app's startup, "Reload and start profiling" is a lifesaver.

!["Start profiling" button.](https://example.com/image.png)

In our sample app, click both buttons a few times and observe real-time feedback from the DevTools highlighting components that are being re-rendered. The screenshots reflect a profile produced by first pressing "Press one" twice and then "Press two" twice, producing four subsequent "commits" by the React renderer.

You can learn more about React's render and commit process in the official docs.

![A picture of React Profiler showing a flame graph representation of React components rendered and re-rendered throughout the profiling session](https://example.com/image.png)

You can press the green View component on the flame graph to "zoom in" to its child components. It's an essential UX paradigm implemented by the Profiler, and hopefully, you'll learn to enjoy it soon. In this case, you'll notice that both Buttons are rendered in every commit, which seems redundant because no props are changing. But are they?

To get an alternative view of the situation, we can access the Ranked view, showing us the components from slowest (yellow) to fastest (green). It's pretty similar to a "Bottom-up" view you may know from other profiling tools, such as Chrome DevTools or native profilers.

![A picture of React Profiler showing a Ranked representation of React components rendered and re-rendered throughout the profiling session](https://example.com/image.png)

In each view, we can see that a Button component is rendered twice, which reflects our two buttons getting re-rendered with every state update.

Looking at our code, this is expected—unless we're using React Compiler, which would automatically memoize this code to produce fewer re-renders. So, let's memoize it by hand.

```jsx
// wrap inline functions with `useCallback`
const onPressHandler = useCallback(() => setCount(count + 1), [count]);
const secondHandler = useCallback(() => setSecond(second + 1), [second]);
// ...
<Button onPress={onPressHandler} title="Press one" />
<Button onPress={secondHandler} title="Press two" />
// wrap Button component with `memo`
const Button = memo(({onPress, title}) => {
  // ...
});
```

After wrapping Button with React.memo and its onPress handlers with React.useCallback, we get a different picture from the profiler, showing the same number of 4 React commits. However, now, only the button that's actually pressed is re-rendered, while the other one gets memoized (with auto-generated _c2 name), which is indicated by the green color of this component and its children.

![A flame graph of the same flow but no with one of the buttons not being updated due to memoization](https://example.com/image.png)

Now, you're well-equipped to start your adventure with profiling any React application—whether it's a web app or mobile app. We strongly advise you to take a break from reading now and try it in the apps you develop on a daily basis. You'll notice the flame graph output will be much noisier due to the bigger complexity of your React app.

> Remember to focus on the components marked with yellow as the ones where React spends the most time. And leverage the "Why did this render?" information.

### Profiling JavaScript Code

React Native DevTools provide great utilities for profiling not only React but also JavaScript code. After all, it's based on Chrome DevTools, reusing its backend. In this section, we will focus on profiling the CPU, examining what's on the call stack, and determining how you can detect long-running operations. If you want to inspect memory-related issues, you can check the How to Hunt JS Memory Leaks chapter.

After launching React Native DevTools, you can go to the "JavaScript Profiler" Tab and start recording the JavaScript CPU Profile.

> If you don't see the "JavaScript Profiler" tab, navigate to settings by clicking on the gear icon at the top right, and from the experiments section, enable JavaScript Profiler.

At the top left, you can select the preferred view. You can choose from Chart, Heavy (Bottom-Up), or Tree. Depending on what you want to achieve, experiment with each view.

Let's experiment with this long-running function:

```javascript
const longRunningFunction = () => {
  let i = 0;
  while (i < 1000000000) {
    i++;
  }
};
```

Start recording the profile and stop once it's finished. Once the profile is created, we can see that the longRunningFunction took over 12 seconds to run, combined with the whole call stack, which led to calling this function. This effectively makes our JS part of the app unresponsive for those 12 seconds, but the native UI part should still be responsive due to React Native's threading model.

> In mobile apps, we want our functions never to block the JS thread for more than 16 ms to achieve 60 FPS and, ideally, 8 ms for 120 FPS, which is getting more popular. Read more about FPS in the How to Measure JS FPS chapter.

![Profiler output showing a flame chart with long-running longRunningFunction taking over 12 seconds to run](https://example.com/image.png)

If the Chart (or flame graph, as React DevTools calls it) information is hard to read, you can try other views, such as Heavy (Bottom Up), where you'll be able to sort by the longest or shortest running function calls.

![Heavy (Bottom Up) view of the same profile information, longRunningFunction is at the top](https://example.com/image.png)

Profiler makes it easier to spot functions that run longer than expected, tracking them down with surgical precision and realizing where a function gets called. This superpower is worth leveraging in your day-to-day work as a software engineer.

## GUIDE: HOW TO HUNT JS MEMORY LEAKS

When a program is executed on a target device or virtual machine, it always occupies some space in the device's Random Access Memory (RAM), which is allocated specifically for that program to execute correctly. When that memory allocation is unexpectedly retained, we say it's "leaking"; hence, we deal with memory leaks.

Memory leaks can be hard to trace without the right tools. In most cases, they arise from programmer errors. If your app constantly uses more and more memory without releasing it, you most likely have a leak somewhere. You might try to play the guessing game of what is leaking, but it's way easier to profile your app with the right tools that will help you find it.

Let's start by defining what memory leaks are.

### Anatomy of a memory leak in a JavaScript app

JavaScript engines, such as Hermes—and any other interpreted languages that leverage a virtual machine to execute—implement a special program called garbage collector (GC). As the name implies, it "collects garbage", where "garbage" refers to memory addresses that are no longer needed for the application to run and can be freed for other programs to use. GC periodically scans allocated objects and determines if an object is no longer needed. Fun fact: Hermes' garbage collector is called Hades.

Although garbage collectors use advanced techniques to reliably and safely determine which memory to free, in some cases, your code may sometimes trick the GC into keeping an object allocated when it shouldn't be. That's how you end up with memory leaks that are often hard to track without dedicated tools.

> In short, a memory leak happens when a program fails to release the memory that is no longer needed.

Here are a few examples of code that leaks memory for better understanding.

1. Listeners that don't stop listening:
```javascript
const BadEventComponent = () => {
  useEffect(() => {
    const subscription = EventEmitter.addListener('myEvent', handleEvent);
    // Missing cleanup function
  }, []);
  return <Text>Listening for events...</Text>;
};
```

2. Timers that don't stop counting:
```javascript
const BadEventComponent = () => {
  useEffect(() => {
    const subscription = EventEmitter.addListener('myEvent', handleEvent);
    // Missing cleanup function
  }, []);
  return <Text>Listening for events...</Text>;
};
```

3. Closures that retain a reference to large objects:
```javascript
class BadClosureExample {
  private largeData: string[] = new Array(1000000).fill('some data');

  createLeakyFunction(): () => number {
    // Entire largeData array is captured in closure
    return () => this.largeData.length;
  }
}

// Fixed
class GoodClosureExample {
  private largeData: string[] = new Array(1000000).fill('some data');

  createEfficientFunction(): () => number {
    // Only capture the length, not the entire array
    const length = this.largeData.length;
    return () => length;
  }
}
```

Now that you know what typical memory leaks look like, let's see what tools you can use to spot them easily.

### Hunting memory leaks with React Native DevTools

The new React Native DevTools builds on top of Chrome DevTools, so if you are familiar with web debugging, you should feel right at home. However, if you want to learn more about Chrome DevTools, you should check out their official documentation.

React Native DevTools offers three ways to profile your app's memory:
- **Heap snapshot** — shows memory distribution among your page's JavaScript objects and related DOM objects.
- **Allocation instrumentation on the timeline** — this profile type shows instrumented JavaScript memory allocations over time. Once a profile is recorded, you can select a time interval to see objects that were allocated within it and still live by the end of the recording. Use this profile type to isolate memory leaks.
- **Allocation sampling** — records memory allocations using the sampling method. This profile type has minimal performance overhead and can be used for long-running operations. It provides good approximations of allocations broken down by the JavaScript execution stack.

We'll focus on Allocation instrumentation on the timeline, which should help us find allocations that are not being freed up. Let's run it on this piece of code:

```javascript
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
  const newDataLeak = generateLargeData(); // Creates large data array
  return () => {
    newDataLeak.forEach((data) => data.id); // Closure captures newDataLeak
  };
};
// Create many closures that each capture their own large data
const createManyLeaks = () => {
  for (let i = 0; i < 10; i++) {
    const leakyClosure = createClosure();
    leakyClosures.push(leakyClosure); // Store reference to prevent GC
  }

  // Trigger all closures to keep data "active"
  leakyClosures.forEach(closure => closure());
};
```

Code showing the third issue from the previous section (closures retaining reference to large data)

Once you launch DevTools, look for the "Memory" tab and select "Allocation instrumentation on timeline". Then, you can click "Start" at the bottom.

![Memory panel in React Native DevTools](https://example.com/image.png)

You have a leak when you finish profiling, and an object is still allocated in memory! But as mentioned before, GC runs periodically, so your objects might not get deallocated instantly. You might need to allocate something else to trigger the deallocation of existing objects.

Here is what the profiler looks like after capturing a snapshot:

![Output from the Allocation instrumentation tool showing a potential memory leak with blue bars](https://example.com/image.png)

There are a few things to keep in mind about the Allocation instrumentation tool:
- Blue bars at the timeline represent allocations.
- If the blue bar turns grey, the object is deallocated.
- Below, we have a list of list of constructors that were called. Right next to it, there are two metrics:
  - Shallow size — the size of memory that is held by the object.
  - Retained size — the memory size that will be freed after the object is deleted.

As you can see, over 30 seconds and multiple calls to the createManyLeaks function, GC cleared only some objects from the memory (shown on the graph as grey), but the blue parts are still alive. This graph screams that multiple leaks are happening.

Let's focus on the first spike and investigate what's causing this.

![Focused view of allocation spike](https://example.com/image.png)

Now that the graph is focused on the first spike, we can see which constructors are being called. Let's expand the Function constructor and see what we have there.

![The discrepancy between shallow and retained size for JSFunction](https://example.com/image.png)

As you can see, there is a call to the JSFunction() constructor whose shallow size is 40 bytes, but the retained size is 2 772 088 bytes(!). It's the leak that we were looking for! This closure is tricking GC into thinking that the objects are still relevant when they are not. DevTools even show us the exact line where this constructor was called.

After finding the leak, you should fix it and re-test. Let's alter the createManyLeaks function and comment-out pushing closures to an array to prevent retaining them.

```javascript
const createManyLeaks = () => {
  for (let i = 0; i < 5; i++) {
    const leakyClosure = createLeak();
    leakyClosure(); // just call it
    // leakyClosures.push(leakyClosure);
    // leakyClosures.forEach(closure => closure());
  }
}
```

As you can see, all the bars are now grey (except the last one, which wasn't yet deallocated), which means there are no leaks!

![Memory allocation profile with no leaks](https://example.com/image.png)

Now you know how to track down and fix memory leaks! In the next chapter, we will focus on Controlled Components.

## GUIDE: HOW TO MEASURE JS FPS

Mobile users expect smooth, well-designed interfaces that respond instantly and provide clear visual feedback. As a result, applications often include numerous animations that must run alongside other processes, such as data fetching and state management. If your application fails to provide a responsive interface, lags user input, or feels "janky" at times, it's a sign the UI is "dropping frames". Users hate it.

### What is FPS

Getting your code to display anything on the screen involves compiling the code to an executable format, which, using various techniques, draws pixels on the screen. A single drawing is called a "frame". If you want your UI not to be static—which you do—your app will need to draw many frames every second. Most mobile devices have a refresh rate of 60 Hz, meaning they can display 60 frames per second, or 60 FPS. That's our metric.

![A difference between 30 and 90 FPS](https://example.com/image.png)

Most people perceive 60 FPS as smooth motion. However, as technology advances, users have access to screens that can refresh 120 or even 240 times a second. Once a user enjoys a 120 Hz screen, chances are they'll consider 60 Hz "choppy," "laggy," or "slow". It means that our target of 16,6 ms to render a frame (1/60 s) is moving, and we should aim for 8,3 ms per frame.

> If you fail to think about your app's performance and choose the right tools to tackle this challenge, you're on the right track to dropping frames sooner or later.

### React Perf Monitor

Thankfully, React Native comes with a handy tool called React Perf Monitor, which can display FPS live as an overlay on top of your app. You can open it by selecting it from the React Native Dev Menu.

> You can open DevMenu by shaking the device or by using dedicated keyboard shortcuts:
> - iOS Simulator: Ctrl + Cmd ⌘ + Z (or Device> Shake).
> - Android emulators: (Cmd ⌘ / Ctrl )+ M.

![React Native Dev Menu drawer](https://example.com/image.png)

![Perf Monitor visible in the top-left corner](https://example.com/image.png)

It opens a pane in the upper left corner of the application. You can hide it with the "Hide Perf Monitor" option by accessing the React DevMenu once again.

This allows you to monitor memory usage and when the FPS slows down. There are actually two FPS monitors—one for the UI (or Main) thread and another for the JS thread. This allows you to quickly realize whether a particular interaction slowdown originates from JavaScript or native.

> Always disable development mode when measuring performance.
> On Android, it's accessible from Dev Menu in Settings > JS Dev Mode.
> On iOS, you'll need to run Metro in dev mode.

As we learned in the How to Profile JS and React Code chapter, when the FPS drops during animation or some operation, you can use a profiler to narrow down the cause.

### Flashlight

FPS in the Dev Menu is easy to see but hard to track and measure. It's quite convenient to get a view of, e.g., the average FPS for a particular interaction or screen load. This is where tools like Flashlight shine. It works only on Android, though.

To get started, make sure you have Flashlight installed and available in your terminal. Open up the Android application you want to measure on a given Android emulator, and then run:​

```
> flashlight measure
```

You will see the diagram with various performance metrics appear in real time as you interact with the application.

> Getting an average FPS is only one of the features available in a lighthouse-like report produced by the tool. The report also includes the app's overall performance score based on the metrics collected during the measurement, such as the average FPS number, average CPU, and RAM usage.

All collected measurements are saved in a JSON file that you can store and compare between different versions of your code. It's a good solution for automated local performance monitoring and to prove that the FPS has improved reliably.

## UNCONTROLLED COMPONENTS

One of the most important performance optimization techniques—and unfortunately also one of the hardest to adopt—is React's uncontrolled components. In this chapter, we'll dive into the world of dynamic UI in a React Native app to compare controlled and uncontrolled component approaches and learn how to migrate from one to the other.

![Picture of two skyscrapers, the less tall (controlled component) and the much taller one (uncontrolled component)](https://example.com/image.png)

For components that you control with JavaScript, we'll reveal why using uncontrolled components can be a game-changer when it comes to making your app feel smoother and more responsive. Even if your app's UI is currently all controlled, we can help you understand this alternative approach and implement it where it makes the most sense.

### Controlled Components in React Native

By default, in React, you typically declare your UI and its behaviors in JavaScript, which allows your React components to fully control their appearance and behavior. This approach has undeniable advantages: it's intuitive as it leverages a declarative programming style, allowing the UI to be a direct reflection of the application state. It's also powerful, as React manages and updates the DOM for you whenever state changes.

In React Native, a controlled component is one whose state is kept in memory in JavaScript and explicitly passed down as props to the native component. For example, if we want to animate a view's position in React Native, we might do something like this:

```jsx
const [animatedValue, setAnimatedValue] = useState(new Animated.Value(0));

useEffect(() => {
  Animated.timing(animatedValue, {
    toValue: 1,
    duration: 1000,
    useNativeDriver: false,
  }).start();
}, []);

return (
  <Animated.View
    style={{
      transform: [
        {
          translateX: animatedValue.interpolate({
            inputRange: [0, 1],
            outputRange: [0, 100],
          }),
        },
      ],
    }}
  />
);
```

In this example, the value of the animation is kept in JavaScript, and React Native needs to communicate that value to the native side on every animation frame. Any change in the animated value in JavaScript triggers a re-rendering of the component on the native side.

If you want to learn more about the threading model of React Native, you can skim through the architectural diagram that was presented in the first section of the guide.

### The Problem with Controlled Components

While controlled components are intuitive and offer great developer ergonomics, they come with a performance cost. Specifically:

1. **JavaScript Thread Overhead**: Each update requires execution on the JavaScript thread, which can become a bottleneck, especially for high-frequency updates like animations or gestures.
2. **Bridge Serialization**: Even when using the new architecture with JSI, there's still serialization overhead when communicating between JavaScript and native.
3. **Competing Tasks**: Your component update competes with other JavaScript tasks, such as processing business logic or handling API responses.

The performance implications of controlled components are particularly evident in animations and gestures. These interactions often require high-frequency updates (up to 60-120 times per second for smooth animations), which can overwhelm the JavaScript thread.

### Uncontrolled Components to the Rescue

An uncontrolled component, on the other hand, is one that maintains its state on the native side, reducing the need for JavaScript thread involvement. This pattern offloads state management from JavaScript to the native UI thread, allowing for smoother animations and gestures.

Take this more performant variant as an example:

```jsx
return (
  <Animated.View
    style={{
      transform: [
        {
          translateX: animatedValue.interpolate({
            inputRange: [0, 1],
            outputRange: [0, 100],
          }),
        },
      ],
    }}
  >
    <Animated.timing
      animatedValue={animatedValue}
      toValue={1}
      duration={1000}
      useNativeDriver={true}
    />
  </Animated.View>
);
```

By setting `useNativeDriver` to `true`, we're offloading the animation calculations to the native UI thread, bypassing JavaScript for every frame update.

### Practical Example: Button with Haptic Feedback

To illustrate the difference between controlled and uncontrolled components, let's implement a button with haptic feedback in both approaches. Imagine we want to create a button that:

1. Changes color when pressed
2. Scales down slightly when pressed
3. Provides haptic feedback when pressed

#### Controlled Implementation

```jsx
import React, {useState} from 'react';
import {Pressable, Text, StyleSheet} from 'react-native';
import * as Haptics from 'expo-haptics';

const ControlledButton = ({title, onPress}) => {
  const [isPressed, setIsPressed] = useState(false);

  return (
    <Pressable
      style={[
        styles.button,
        {
          backgroundColor: isPressed ? '#1e88e5' : '#2196f3',
          transform: [{scale: isPressed ? 0.95 : 1}],
        },
      ]}
      onPressIn={() => {
        setIsPressed(true);
        Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium);
      }}
      onPressOut={() => setIsPressed(false)}
      onPress={onPress}>
      <Text style={styles.text}>{title}</Text>
    </Pressable>
  );
};

const styles = StyleSheet.create({
  button: {
    paddingVertical: 12,
    paddingHorizontal: 24,
    borderRadius: 6,
    justifyContent: 'center',
    alignItems: 'center',
  },
  text: {
    color: 'white',
    fontWeight: 'bold',
  },
});
```

This implementation works but has performance issues:
- Every press causes a state update in JavaScript
- The state update triggers a re-render
- The re-render causes communication over the bridge
- The animation of the scale is handled by JavaScript

#### Uncontrolled Implementation

```jsx
import React from 'react';
import {Pressable, Text, StyleSheet} from 'react-native';
import * as Haptics from 'expo-haptics';
import Animated, {
  useAnimatedStyle,
  useSharedValue,
  withSpring,
} from 'react-native-reanimated';

const AnimatedPressable = Animated.createAnimatedComponent(Pressable);

const UncontrolledButton = ({title, onPress}) => {
  const scale = useSharedValue(1);
  const backgroundColor = useSharedValue('#2196f3');

  const animatedStyle = useAnimatedStyle(() => {
    return {
      backgroundColor: backgroundColor.value,
      transform: [{scale: scale.value}],
    };
  });

  return (
    <AnimatedPressable
      style={[styles.button, animatedStyle]}
      onPressIn={() => {
        scale.value = withSpring(0.95);
        backgroundColor.value = '#1e88e5';
        Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Medium);
      }}
      onPressOut={() => {
        scale.value = withSpring(1);
        backgroundColor.value = '#2196f3';
      }}
      onPress={onPress}>
      <Text style={styles.text}>{title}</Text>
    </AnimatedPressable>
  );
};
```

In this implementation, with react-native-reanimated:
- The animations run on the UI thread
- No state updates in JavaScript are needed
- No re-renders occur when pressing the button
- The haptic feedback still needs JavaScript but doesn't affect the animation

The uncontrolled version provides a much smoother experience, especially on lower-end devices or when the JavaScript thread is busy with other tasks.

### When to Use Uncontrolled Components

While uncontrolled components offer performance benefits, they aren't always the right choice. Consider using them for:

1. **Animations**: Especially those that run at 60fps or higher
2. **Gestures**: For smooth tracking of touch input
3. **Scroll interactions**: When building custom scroll behaviors
4. **Complex, dynamic UIs**: When many elements update simultaneously

Stick with controlled components when:
1. You need the state for business logic
2. The interaction is infrequent (like toggling a switch)
3. The component is simple and doesn't require frequent updates

The key is to identify which parts of your UI benefit most from being uncontrolled and focus your optimization efforts there. It's not unusual for an app to use a mix of both approaches - controlled for most of the UI and uncontrolled for performance-critical interactions.

Most of the uncontrolled component techniques in React Native leverage Reanimated 2, as it provides a powerful API for creating uncontrolled animations. If you're new to Reanimated 2, we recommend checking out their documentation and examples to get started.

# PART 2: NATIVE
## Guides and techniques to improve your React Native app by understanding native platforms

Understanding the underlying native platforms is crucial for optimizing React Native applications. While React Native allows us to write code once and run it on multiple platforms, each platform has its own characteristics, limitations, and performance considerations.

## UNDERSTAND PLATFORM DIFFERENCES

React Native applications leverage the device's native capabilities, but iOS and Android have fundamentally different architectures. Understanding these differences is key to building apps that perform well on both platforms.

### iOS vs. Android

iOS and Android differ in many ways that impact performance:

#### Memory Management

- **iOS**: Uses Automatic Reference Counting (ARC). Memory allocation is generally more efficient, and resource constraints are less common on modern iOS devices.
- **Android**: Uses garbage collection, which can cause frame drops during collection cycles. Android devices have a wider range of hardware capabilities, making memory management more challenging.

#### Rendering Engines

- **iOS**: Uses Core Animation and UIKit, which provides consistent, high-performance rendering.
- **Android**: Uses a variety of rendering engines depending on the version (e.g., Skia, Vulkan). The fragmentation of Android devices can lead to inconsistent rendering performance.

#### Threading Models

- **iOS**: Main thread handles UI updates, with better synchronization between UI and background tasks.
- **Android**: Uses the main thread for UI but has different thread management, which can sometimes cause UI jank if not managed properly.

### Platform-Specific Optimizations

#### iOS-Specific Optimizations

1. **Use Native Components When Possible**:
   
   ```jsx
   // Good practice: leveraging native iOS components
   import { ActionSheetIOS } from 'react-native';
   
   // Use native iOS Action Sheet for better performance
   const showActionSheet = () => {
     ActionSheetIOS.showActionSheetWithOptions(
       {
         options: ['Cancel', 'Delete', 'Save'],
         destructiveButtonIndex: 1,
         cancelButtonIndex: 0,
       },
       (buttonIndex) => {
         if (buttonIndex === 1) {
           deleteItem();
         } else if (buttonIndex === 2) {
           saveItem();
         }
       }
     );
   };
   ```

2. **Optimize for Metal**:
   Metal provides hardware-accelerated rendering on iOS. When using libraries that support Metal (like Skia), you can achieve better performance.

3. **Leverage iOS-Specific APIs**:
   Use platform-specific modules when available.

   ```jsx
   import { Platform } from 'react-native';
   
   const performOptimizedTask = () => {
     if (Platform.OS === 'ios') {
       // Use iOS-specific high-performance API
       return iosSpecificImplementation();
     } else {
       // Fallback for Android
       return genericImplementation();
     }
   };
   ```

#### Android-Specific Optimizations

1. **Handle Configuration Changes**:
   Android devices undergo configuration changes (e.g., rotation) that can trigger component remounts. Use `android:configChanges` in your Android manifest to handle these efficiently.

2. **Optimize ListView Performance**:
   Android can struggle with large lists. Use `FlatList` with proper keys and optimizations.

   ```jsx
   <FlatList
     data={items}
     renderItem={({ item }) => <Item item={item} />}
     keyExtractor={item => item.id}
     maxToRenderPerBatch={10}
     windowSize={5}
     initialNumToRender={5}
     removeClippedSubviews={Platform.OS === 'android'}
   />
   ```

3. **Be Mindful of Android API Levels**:
   Different Android versions support different features. Use feature detection and fallbacks.

   ```jsx
   import { Platform } from 'react-native';
   
   const useModernAnimation = () => {
     if (Platform.OS === 'android' && Platform.Version >= 21) {
       // Use Lollipop+ animations
       return modernAnimation();
     } else {
       return legacyAnimation();
     }
   };
   ```

### Testing Across Platforms

To ensure optimal performance on both platforms:

1. **Test on Real Devices**: Emulators can't fully replicate the performance characteristics of real devices.

2. **Test on Different Device Tiers**: Don't just test on high-end devices. Users with mid-range or budget devices will have different experiences.

3. **Profile Platform-Specific Metrics**:
   - For iOS: Use Instruments to profile CPU, memory, and rendering.
   - For Android: Use Android Profiler to monitor similar metrics.

4. **Use Platform-Specific Development Tools**:
   - For iOS: Xcode's performance tools
   - For Android: Android Studio's profiling tools

### Platform-Specific Code Organization

Organize your code to handle platform differences without cluttering your component logic:

#### Using Platform-Specific File Extensions

```
Button.ios.js
Button.android.js
```

React Native will automatically import the correct file based on the platform.

#### Using Platform Module

```jsx
import { Platform, StyleSheet } from 'react-native';

const styles = StyleSheet.create({
  container: {
    elevation: Platform.OS === 'android' ? 4 : 0,
    shadowColor: Platform.OS === 'ios' ? '#000' : 'transparent',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.3,
    shadowRadius: 4,
  },
});
```

#### Platform-Specific Components

```jsx
import { Platform } from 'react-native';
import AndroidFastImage from './AndroidFastImage';
import IOSFastImage from './IOSFastImage';

const FastImage = Platform.select({
  ios: IOSFastImage,
  android: AndroidFastImage,
});

export default FastImage;
```

### Leveraging Native Modules for Performance

When JavaScript isn't fast enough, consider writing native modules:

```jsx
// JavaScript interface
import { NativeModules } from 'react-native';
const { ExpensiveCalculation } = NativeModules;

// Usage
const result = await ExpensiveCalculation.calculate(data);
```

### Platform-Specific Memory Considerations

#### Android Memory Constraints

Android devices have more varied memory limitations. Use the following strategies:

1. **Implement `onLowMemory` Handling**:
   
   ```java
   // In your MainApplication.java
   @Override
   public void onLowMemory() {
     super.onLowMemory();
     // Clear caches, release non-critical resources
   }
   ```

2. **Monitor Memory Usage**:
   
   ```jsx
   import { NativeModules } from 'react-native';
   const { MemoryInfo } = NativeModules;
   
   // Check memory periodically
   const checkMemory = async () => {
     const { availableMem, totalMem } = await MemoryInfo.getInfo();
     if (availableMem / totalMem < 0.2) {
       // Low memory situation, take action
       clearNonEssentialCache();
     }
   };
   ```

#### iOS Memory Management

iOS memory management is more straightforward but still requires attention:

1. **Release Large Resources When Not in Use**:
   
   ```jsx
   componentWillUnmount() {
     // Release large data structures
     this.videoCache = null;
     this.imageData = null;
   }
   ```

2. **Use Autorelease Pools in Native Code**:
   When writing native iOS modules, use autorelease pools for temporary objects.

### Conclusion

Understanding platform differences is essential for optimizing React Native applications. By leveraging platform-specific features and accounting for each platform's strengths and limitations, you can build applications that perform exceptionally well across both iOS and Android.

In the next section, we'll explore how to optimize native bridge communication, a critical aspect of React Native performance.

# PART 3: BUNDLING
## Techniques to optimize how your code is bundled, delivered and loaded

In the React Native ecosystem, bundling is a critical part of the app deployment process. It's the step where your JavaScript code, along with its dependencies, is processed, minified, and packaged into a bundle that your app can load. The efficiency of this bundle directly impacts your app's startup time, overall performance, and user experience.

## LOAD CODE REMOTELY WHEN NEEDED

In traditional React Native applications, all JavaScript code is bundled together and loaded when the app starts, which can lead to longer startup times and larger app sizes. Remote code loading offers a solution by allowing you to:

1. Load only essential code at startup
2. Fetch additional features on demand
3. Update functionality without a full app release

This approach significantly improves initial load times and allows for more dynamic app experiences.

### The Problem with Monolithic Bundles

When all your code is packed into a single bundle:

1. **Increased Startup Time**: The JavaScript VM must parse and compile all code, even features the user might not need immediately.
2. **Larger App Size**: All features are included in the initial download.
3. **Update Limitations**: Any change requires a full app store release.

### Implementing Remote Code Loading

There are several approaches to loading code remotely in React Native:

#### 1. Using CodePush (AppCenter)

Microsoft's CodePush allows you to deploy updates directly to users' devices:

```jsx
import codePush from 'react-native-code-push';

const codePushOptions = {
  checkFrequency: codePush.CheckFrequency.ON_APP_START,
  installMode: codePush.InstallMode.ON_NEXT_RESTART
};

const App = () => {
  // Your app component
};

export default codePush(codePushOptions)(App);
```

This basic integration will:
- Check for updates when the app starts
- Install any available updates on the next app restart

For more fine-grained control:

```jsx
const checkAndApplyUpdate = async () => {
  try {
    const update = await codePush.checkForUpdate();
    if (update) {
      // Update is available
      Alert.alert(
        'Update Available',
        'An update is available. Would you like to install it now?',
        [
          { text: 'Later', style: 'cancel' },
          { 
            text: 'Install', 
            onPress: async () => {
              const result = await codePush.sync({
                installMode: codePush.InstallMode.IMMEDIATE
              });
            }
          }
        ]
      );
    }
  } catch (error) {
    console.log('CodePush check failed', error);
  }
};
```

#### 2. Using React Native's Dynamic Import

For feature-specific code loading, you can use JavaScript's dynamic `import()`:

```jsx
// Main app code
const App = () => {
  const [FeatureComponent, setFeatureComponent] = useState(null);
  const [isLoading, setIsLoading] = useState(false);

  const loadFeature = async () => {
    setIsLoading(true);
    try {
      // Dynamically import the feature
      const module = await import('./features/AdvancedFeature');
      setFeatureComponent(() => module.default);
    } catch (error) {
      console.error('Failed to load feature:', error);
    } finally {
      setIsLoading(false);
    }
  };

  return (
    <View style={styles.container}>
      <Button title="Load Advanced Feature" onPress={loadFeature} />
      {isLoading && <ActivityIndicator size="large" />}
      {FeatureComponent && <FeatureComponent />}
    </View>
  );
};
```

This approach works well for features that aren't needed immediately but may be used later in the user journey.

#### 3. Using React Suspense with Lazy Loading

React's Suspense and lazy loading can provide a cleaner way to implement dynamic imports:

```jsx
import React, { Suspense, lazy } from 'react';
import { View, ActivityIndicator } from 'react-native';

// Lazy load the component
const HeavyFeature = lazy(() => import('./features/HeavyFeature'));

const App = () => {
  const [showFeature, setShowFeature] = useState(false);

  return (
    <View style={styles.container}>
      <Button 
        title="Show Heavy Feature" 
        onPress={() => setShowFeature(true)} 
      />
      
      {showFeature && (
        <Suspense fallback={<ActivityIndicator size="large" />}>
          <HeavyFeature />
        </Suspense>
      )}
    </View>
  );
};
```

### Creating Separate Bundles for Remote Loading

To fully leverage remote code loading, you need to create separate bundles for different features. This can be achieved using Metro configuration:

```js
// metro.config.js
module.exports = {
  transformer: {
    getTransformOptions: async () => ({
      transform: {
        experimentalImportSupport: true,
        inlineRequires: true,
      },
    }),
  },
  serializer: {
    createModuleIdFactory: () => {
      // Custom ID factory for consistent bundle IDs
    },
    // Configure multiple entry points
    processModuleFilter: (module) => {
      return true;
    },
  },
};
```

Then, you can create separate entry points for each feature bundle:

```
// Project structure
/src
  /features
    /feature1
      index.js (entry point)
    /feature2
      index.js (entry point)
  App.js (main entry point)
```

And build multiple bundles:

```
npx react-native bundle --platform ios --dev false --entry-file src/features/feature1/index.js --bundle-output ./bundles/feature1.ios.bundle
npx react-native bundle --platform ios --dev false --entry-file src/features/feature2/index.js --bundle-output ./bundles/feature2.ios.bundle
```

### Serving Remote Bundles

Once you have separate bundles, you need a way to serve them:

1. **Static Hosting**: Upload bundles to a CDN like Amazon S3, Google Cloud Storage, or similar services.
2. **Dynamic Delivery API**: Create an API that serves the appropriate bundle based on app version, user segment, or feature flags.

Example fetch implementation:

```jsx
const loadRemoteBundle = async (bundleName) => {
  try {
    const bundleUrl = `https://your-cdn.com/bundles/${bundleName}.bundle`;
    
    // Download the bundle
    const response = await fetch(bundleUrl);
    const bundleText = await response.text();
    
    // Execute the bundle (simplified example)
    // Warning: Evaluate code safely to avoid security risks
    // Consider using a more robust solution like CodePush
    const moduleFactory = new Function('require', 'module', 'exports', bundleText);
    
    // Create a module context
    const module = { exports: {} };
    moduleFactory(require, module, module.exports);
    
    return module.exports;
  } catch (error) {
    console.error('Failed to load remote bundle:', error);
    throw error;
  }
};

// Usage
const FeatureModule = await loadRemoteBundle('feature1');
```

### Security Considerations

Remote code loading introduces security concerns that need to be addressed:

1. **Code Integrity**: Ensure bundle integrity with checksums or signatures.
2. **HTTPS**: Always use HTTPS for downloading bundles.
3. **Code Execution**: Be cautious with `eval`-like functionality; prefer established solutions like CodePush.

Example with basic security:

```jsx
const loadRemoteBundle = async (bundleName) => {
  try {
    const bundleUrl = `https://your-cdn.com/bundles/${bundleName}.bundle`;
    const checksumUrl = `https://your-cdn.com/bundles/${bundleName}.checksum`;
    
    // Fetch both bundle and its checksum
    const [bundleResponse, checksumResponse] = await Promise.all([
      fetch(bundleUrl),
      fetch(checksumUrl)
    ]);
    
    const bundleText = await bundleResponse.text();
    const expectedChecksum = await checksumResponse.text();
    
    // Verify bundle integrity
    const actualChecksum = calculateSHA256(bundleText);
    if (actualChecksum !== expectedChecksum) {
      throw new Error('Bundle integrity check failed');
    }
    
    // Now safe to execute...
  } catch (error) {
    console.error('Bundle loading failed:', error);
    throw error;
  }
};
```

### Testing Remote Bundles

To ensure reliability, test your remote loading implementation thoroughly:

1. **Offline Testing**: Test behavior when network is unavailable.
2. **Version Compatibility**: Ensure main app and feature bundles are compatible.
3. **Rollback Strategy**: Have a plan for reverting to older bundle versions if issues arise.

### Benefits of Remote Code Loading

When implemented correctly, remote code loading provides:

1. **Faster Initial Launch**: Only essential code is loaded at startup.
2. **Reduced App Size**: Features can be downloaded only when needed.
3. **Flexible Updates**: Critical fixes can be pushed without app store approval.
4. **Feature Flagging**: Enable/disable features remotely.
5. **A/B Testing**: Deploy different code paths to different user segments.

### Production Recommendations

For production apps, consider these best practices:

1. **Use Established Solutions**: Prefer mature tools like CodePush rather than building custom solutions.
2. **Implement Fallbacks**: Have local copies of critical bundles.
3. **Gradually Roll Out**: Deploy remote bundles to increasing percentages of users.
4. **Monitor Performance**: Track bundle load times and failure rates.
5. **Version Management**: Maintain compatibility between app versions and bundles.

### Conclusion

Remote code loading is a powerful technique for optimizing React Native applications, providing faster startup times and more flexible deployment options. While it requires careful implementation and consideration of security implications, the performance benefits make it worthwhile for many applications.

## REDUCE BUNDLE SIZE

The size of your JavaScript bundle has a direct impact on your app's performance. Larger bundles take longer to download, parse, and execute, which can significantly slow down your app's startup time. In this section, we'll explore various strategies to reduce your bundle size.

### Why Bundle Size Matters

A bloated bundle can negatively impact your app in several ways:

1. **Slower Startup Time**: Larger bundles take longer to load, parse and execute.
2. **Higher Memory Usage**: More code in memory means less room for other operations.
3. **More Network Usage**: Increased data consumption for users with limited data plans.
4. **Higher Battery Consumption**: More processing required to parse and execute code.

### Measuring Your Bundle Size

Before optimizing, you need to understand what's contributing to your bundle size. Here are tools to help:

#### Using Metro Bundle Analyzer

The Metro bundler has built-in support for analyzing your bundle:

```bash
npx react-native bundle --dev false --platform ios --entry-file index.js --bundle-output /tmp/bundle.js --stats-output /tmp/stats.json
```

Then visualize the stats:

```bash
npx metro-visualizer --stats-path /tmp/stats.json
```

#### Using Source Map Explorer

For a more detailed breakdown:

```bash
# Install source-map-explorer
npm install -g source-map-explorer

# Generate bundle with source map
npx react-native bundle --dev false --platform ios --entry-file index.js --bundle-output /tmp/bundle.js --sourcemap-output /tmp/sourcemap.js

# Analyze
source-map-explorer /tmp/bundle.js /tmp/sourcemap.js
```

This will open a treemap visualization showing the size of each module in your bundle.

### Strategies to Reduce Bundle Size

#### 1. Remove Unused Dependencies

Dependencies are often the biggest contributors to bundle size. Remove any packages you're not using:

```bash
# Find unused dependencies
npx depcheck

# Remove them
npm uninstall package-name
```

For partially used large libraries, consider alternatives:

```jsx
// Instead of importing the entire library
import { map, filter, reduce } from 'lodash';

// Import only what you need
import map from 'lodash/map';
import filter from 'lodash/filter';
import reduce from 'lodash/reduce';

// Or use native alternatives
const map = (array, callback) => array.map(callback);
```

#### 2. Use Tree Shaking

Modern JavaScript bundlers can eliminate unused code through tree shaking:

```js
// metro.config.js
module.exports = {
  transformer: {
    getTransformOptions: async () => ({
      transform: {
        experimentalImportSupport: true, // Enable tree shaking
        inlineRequires: true,
      },
    }),
  },
};
```

Ensure your code is structured to be tree-shakable:

```jsx
// ❌ Bad: Exports everything as a single object (not tree-shakable)
const utilities = {
  formatDate: (date) => { /* ... */ },
  formatCurrency: (amount) => { /* ... */ },
  calculateTotal: (items) => { /* ... */ }
};
export default utilities;

// ✅ Good: Named exports (tree-shakable)
export const formatDate = (date) => { /* ... */ };
export const formatCurrency = (amount) => { /* ... */ };
export const calculateTotal = (items) => { /* ... */ };
```

#### 3. Code Splitting

Split your code into smaller chunks that can be loaded on demand:

```jsx
import React, { lazy, Suspense } from 'react';

// Instead of: import HeavyComponent from './HeavyComponent';
const HeavyComponent = lazy(() => import('./HeavyComponent'));

const App = () => (
  <Suspense fallback={<Loading />}>
    {shouldShowHeavyComponent && <HeavyComponent />}
  </Suspense>
);
```

#### 4. Use Smaller Alternatives for Large Libraries

Some popular libraries have lighter alternatives:

```jsx
// ❌ Heavy: moment.js (large bundle size)
import moment from 'moment';
const formattedDate = moment(date).format('YYYY-MM-DD');

// ✅ Light: date-fns (tree-shakable, much smaller)
import { format } from 'date-fns';
const formattedDate = format(date, 'yyyy-MM-dd');

// ✅ Lightest: Native JavaScript
const formattedDate = new Date(date).toISOString().split('T')[0];
```

#### 5. Optimize Images and Assets

Assets can significantly contribute to bundle size:

```jsx
// Use appropriate image formats and sizes
import smallImage from './assets/small.webp'; // WebP instead of PNG
import { Image } from 'react-native';

// Use resolution-specific images
const image = Platform.select({
  ios: require('./assets/image.ios.png'),
  android: require('./assets/image.android.png'),
});

// For web-specific optimizations (React Native Web)
const Picture = Platform.select({
  web: ({ sources, fallback, ...props }) => (
    <picture>
      {sources.map((source) => (
        <source {...source} key={source.srcSet} />
      ))}
      <img src={fallback} {...props} />
    </picture>
  ),
  default: Image,
});
```

#### 6. Use Hermes Engine

Hermes, React Native's optimized JavaScript engine, produces smaller, more efficient bytecode:

```js
// android/app/build.gradle
def enableHermes = true // Enable Hermes

// iOS: use_react_native!
use_react_native!(
  :path => config[:reactNativePath],
  :hermes_enabled => true
)
```

Hermes bytecode is typically smaller than JavaScript, reducing the overall size impact.

#### 7. Configure Metro for Production

Ensure your Metro configuration is optimized for production:

```js
// metro.config.js
module.exports = {
  transformer: {
    minifierConfig: {
      keep_classnames: false, // Set to true only if needed
      keep_fnames: false, // Set to true only if needed
      mangle: {
        toplevel: true,
        reserved: [],
      },
      compress: {
        // Optimization settings
        unused: true,
        dead_code: true,
        global_defs: {
          __DEV__: false,
        },
      },
    },
  },
};
```

#### 8. Use Production Builds

Always test with production builds, not development ones:

```bash
# iOS
npx react-native run-ios --configuration Release

# Android
npx react-native run-android --variant=release
```

#### 9. Analyze and Trim Polyfills

React Native includes polyfills that might not be needed:

```js
// babel.config.js
module.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  plugins: [
    ['transform-remove-console', { exclude: ['error', 'warn'] }] // Remove console.logs
  ],
};
```

#### 10. Implement Dead Code Elimination

Remove code paths that are never executed in production:

```jsx
if (__DEV__) {
  // This code will be removed in production builds
  validateProps(props);
  monitorPerformance();
}

// Platform-specific code
if (Platform.OS === 'ios') {
  // iOS-only code
} else {
  // Android-only code
}
```

### Real-World Example: Bundle Size Reduction

Let's look at a step-by-step example of reducing a React Native app's bundle size:

#### Initial State

- JS Bundle Size: 8.2 MB
- App Launch Time: 3.5 seconds

#### Step 1: Analyze Bundle

```bash
npx react-native bundle --dev false --platform ios --entry-file index.js --bundle-output /tmp/bundle.js --stats-output /tmp/stats.json
npx metro-visualizer --stats-path /tmp/stats.json
```

Analysis showed:
- Moment.js: 1.2 MB
- Unused components from UI library: 2.3 MB
- Large SVG assets: 0.8 MB
- Redux dev tools: 0.3 MB

#### Step 2: Implement Fixes

1. Replace Moment.js with date-fns:
```jsx
// Before
import moment from 'moment';
const formattedDate = moment(date).format('YYYY-MM-DD');

// After
import { format } from 'date-fns';
const formattedDate = format(date, 'yyyy-MM-dd');
```

2. Import UI components selectively:
```jsx
// Before
import { Button, Card, Text, /* and 30 other components */ } from 'ui-library';

// After
import Button from 'ui-library/Button';
import Card from 'ui-library/Card';
import Text from 'ui-library/Text';
```

3. Convert SVGs to optimized PNGs or use react-native-svg efficiently:
```jsx
// Before: Large imported SVG files
import Logo from './assets/logo.svg';

// After: Optimized component
import Svg, { Path } from 'react-native-svg';

const Logo = (props) => (
  <Svg width={24} height={24} viewBox="0 0 24 24" {...props}>
    <Path d="M12 2L2 7l10 5 10-5-10-5z" fill="#4285F4" />
  </Svg>
);
```

4. Remove development tools in production:
```jsx
// configureStore.js
const configureStore = () => {
  const middlewares = [thunk];
  
  if (__DEV__) {
    const { logger } = require('redux-logger');
    middlewares.push(logger);
  }
  
  return createStore(rootReducer, applyMiddleware(...middlewares));
};
```

#### Step 3: Measure Results

- New JS Bundle Size: 3.6 MB (56% reduction)
- New App Launch Time: 1.8 seconds (49% faster)

### Advanced Optimization Techniques

For projects requiring extreme optimization:

#### 1. Custom Babel Plugins

Create custom Babel plugins to transform or remove code patterns:

```js
// babel-plugin-transform-remove-unused-imports.js
module.exports = function() {
  return {
    visitor: {
      ImportDeclaration(path, state) {
        // Custom logic to detect and remove unused imports
      }
    }
  };
};
```

#### 2. Bytecode Optimization

For Hermes-powered apps, optimize at the bytecode level:

```bash
# Generate Hermes bytecode
npx react-native bundle --platform android --dev false --entry-file index.js --bundle-output index.android.bundle --assets-dest android/app/src/main/res/

# Compile to bytecode
node_modules/hermes-engine/osx-bin/hermesc -emit-binary -out=index.android.hbc index.android.bundle -O
```

#### 3. Differential Bundling

Serve different bundles based on device capabilities:

```js
// metro.config.js
module.exports = {
  resolver: {
    resolverMainFields: ['modern', 'browser', 'main'],
  },
  // ...other config
};
```

### Bundle Size Monitoring

Set up continuous monitoring for bundle size regressions:

```jsx
// In CI pipeline
const currentBundleSize = getFileSizeInBytes('bundle.js');
const previousBundleSize = getPreviousBundleSize();

if (currentBundleSize > previousBundleSize * 1.05) { // 5% increase threshold
  console.error('Bundle size increased by more than 5%!');
  process.exit(1);
}
```

### Best Practices Summary

1. **Measure First**: Use bundle analyzers to identify the largest contributors.
2. **Target Big Wins**: Focus on dependencies first, as they often account for 70%+ of bundle size.
3. **Be Selective with Imports**: Import only what you need from libraries.
4. **Enable Tree Shaking**: Structure code to be tree-shakable and enable relevant bundler options.
5. **Use Code Splitting**: Load non-critical features on demand.
6. **Optimize Assets**: Compress images and use appropriate formats.
7. **Enable Hermes**: Use React Native's optimized JavaScript engine.
8. **Monitor Continuously**: Set up bundle size tracking in your CI pipeline.

By implementing these strategies, you can significantly reduce your React Native app's bundle size, resulting in faster startup times and a more responsive user experience.

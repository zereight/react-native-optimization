React Native 
Optimization
the Ultimate Guide 

TABLE OF CONTENTS
Introduction 
Preface  4
How to Read This Book 5
Why Performance Matters 7
Part 1: JavaScript
How to Profile JS and React Code 14
How to Measure JS FPS 21
How to Hunt JS Memory Leaks 24
Uncontrolled Components 30
Higher-Order Specialized Components 34
Atomic State Management 42
Concurrent React 46
React Compiler 52
High-Performance Animations Without Dropping Frames 59
Part 2: Native
Understand Platform Differences 67
How to Profile Native Parts of React Native 76
How to Measure TTI 85
Understanding Native Memory Management 93
Understand the Threading Model of Turbo Modules and Fabric 105
Use View Flattening 113
Use dedicated React Native SDKs Over Web 117
Make Your Native Modules Faster 122
How to Hunt Memory Leaks 130

Part 3: Bundling 
How to Analyze JS Bundle Size 142
How to Analyze App Bundle Size 148
Determine True Size of Third-Party Libraries 154
Avoid Barrel Exports 156
Experiment With Tree Shaking 159
Load Code Remotely When Needed 163
Shrink Code With R8 Android 167
Use Native Assets Folder 170
Disable JS Bundle Compression 175
Acknowledgements 
About The Authors 177
Libraries and Tools Mentioned in This Guide 179

PREFACE 
React Native has come a long way. From the early days as a promising experiment in cross-
platform development to adoption by some of the world's largest enterprises, its evolution has 
been nothing short of remarkable. The framework has matured, proving capable of handling 
the demands of high-scale applications while continuing to refine and push its technical 
boundaries.
For years, we have seen React Native introduce new features, often groundbreaking, with 
every release. Today, we see steady, low-level improvements that often enhance performance, 
stability, and scalability. These refinements reflect how React Native is being adopted more 
widely, reinforcing its role as a mature and enterprise-ready framework.
A significant milestone in this journey was the introduction of the New Architecture, which 
redefined many core aspects of the framework, including how we think about development in 
general and performance optimizations. It brought new features and best practices, enabling 
developers to build even more efficient and scalable applications. 
And so, we decided it was time to overhaul our guide to optimization completely. The landscape 
has changed, and so must our approach. Some techniques that were once essential are no 
longer relevant; others have taken on new importance. In this new edition, we aim to equip 
developers with the latest insights, tools, and strategies to make the most of React Native's 
evolving capabilities.
Whether you're an experienced React Native engineer or just getting started, this guide goes 
beyond performance best practices. It also provides essential knowledge to help you understand 
React Native in general, including what happens under the hood. We rely on that knowledge 
every day to build better, more efficient, and scalable applications, and we hope it will help you 
do the same.
— Mike Grabowski, Callstack's Founder & CTO

HOW TO READ THIS BOOK
This book is intended for React Native developers of various levels of experience. We believe 
that both newcomers and seasoned React Native devs, regardless of whether they come from 
a web or native app development background, will find something valuable and applicable in 
their apps. 
We acknowledge that at a given time you may not be interested in all the optimizations presented 
in this book. Oftentimes, your focus will be on a specific area, such as optimizing React re-
renders. That's why, although you can read the whole book linearly, we made sure it's easy to 
open on any given chapter and get right to the topic of your interest.
What's waiting for you inside?
To make it easier for everyone to find relevant information, we split this guide into three 
parts focusing on different kinds of optimization you may be particularly interested in: the 
React side, the Native side, and overall build-time optimizations. All three parts contain an 
introduction to the main topic, detailed guides, and best practices to help you improve the 
most important metrics describing your app's performance: FPS (Frames Per Second) and TTI 
(Time to Interactive).
Here's what you can expect in each part:
 ■Introduction—where we present key topics, terms, and ideas related to the main 
topic.
 ■Guides—where we explain how to use specialized tools and measure important 
metrics.
 ■Best practices—where we show you what to do to make your app run and initialize 
faster.

Conventions used in this book
To better illustrate the ideas presented in this book, we've included a lot of short code snippets, 
screenshots from the tools we use, and diagrams. 
We link to external resources, such as official docs or libraries, but also to other chapters of this 
book.
To give you extra context on certain topics, we decided to put them in callouts—while not 
intended for linear reading, we highly recommend not skipping those when reading.
About Callstack
At Callstack, we are committed to advancing React Native and empowering developers to 
build high-performance applications. As core contributors and Meta partners, we work 
closely with the community—shaping proposals, maintaining key modules, and driving the 
framework's evolution. Our team actively contributes to React Native's monthly releases, 
ensuring developers have access to the latest improvements. By sharing our expertise, tools, 
and best practices, we help teams solve performance challenges, optimize their apps, and push 
the boundaries of what's possible with React Native.
Let's get in touch
React Native thrives because of its community, and you can be part of its evolution. Whether 
by optimizing your own apps, contributing to open-source projects, or staying informed about 
the latest advancements, your involvement helps push the ecosystem forward.
Here's how you can stay engaged:
 ■Stay up to date—subscribe to our newsletter and follow our latest insights, open-
source contributions, and best practices.
 ■Join the conversation—Join our Discord server to discuss proposals, share your ideas, 
and help shape the future of React Native.
 ■Contribute—explore and collaborate on our open-source projects.
And if you want to get in touch, contact us  anytime. Let's build a faster, better React Native 
together!

WHY PERFORMANCE MATTERS
When building mobile apps, performance is not just a technical concern—it's a user experience 
priority. A fast, responsive app can make the difference between a delighted user and one who 
abandons your app entirely. But what does "fast" really mean? Is it about raw numbers or 
maybe a user's perception?
The user's perspective
Perceived performance is all about how fast your app feels to the user. It's not just about raw 
numbers or benchmarks—it's about creating the illusion of speed. 
In the 1940s, an office building in New York faced complaints about slow elevators. To 
address this, instead of speeding up the elevators, the building manager installed mirrors near 
the elevators. This simple change distracted people by giving them something to do (look at 
themselves), reducing perceived wait times, and effectively solving the issue. While this story 
may be more of an urban legend, the concept itself is rooted in valid psychological principles 
about perceived time and distraction that influence the feeling of how fast a particular event is.
When a mobile app takes a few seconds to load, showing a splash screen, skeleton UI, or even 
a game to play can make users feel like the app is responsive and ready to use. That's why perceived 
performance is often more important than actual performance in shaping user satisfaction.
However, focusing solely on perceived performance can be misleading. While tricks like 
animations, placeholders, or preloading content can improve the user's perception, they don't 
address the underlying performance issues. That's where measurable metrics like TTI and FPS 
show their value.
The metrics that matter: TTI and FPS
Out of the many metrics you can measure and monitor, two have the most impact on how 
users perceive the speed of your app. It's how quickly they can interact with the app described 
by TTI and how fluid the app feels when interacting with it described by FPS at a given time.

Time to Interactive (TTI)
Measuring the app's boot-time performance is described by the TTI metric. It measures 
how quickly your app becomes usable after launch. This is the moment when users can start 
interacting with the app without delays or jank. But TTI isn't just about the app's initial load. 
Companies often track variations of this metric, such as Time to Home (how long it takes to 
load the home screen) or Time to Specific Screen (how long it takes to navigate to a key feature). 
These metrics are especially important for apps with complex navigation flows or heavy initial 
data fetching.
If your app takes too long to load, users may abandon it before they even get a chance to explore 
its features or simply get fed up with it being slow anytime they try to do something, making 
them feel miserable and starting to associate your app, and sometimes even its brand, with that 
feeling.
Frames Per Second (FPS)
Once your app is up and running, FPS becomes the key metric for runtime performance. FPS 
measures how smoothly your app responds to user interactions, such as scrolling, swiping, or 
tapping buttons. A high FPS (ideally 60 frames per second or more) ensures that animations 
and transitions feel smooth and natural. When FPS drops, user experience lags, and animations 
begin to stutter, making your app feel unpolished or slow to respond. This is especially critical 
for apps with rich visual content or complex interactions.
What does fast even mean?
As much as we love data and a scientific approach to validating our hypotheses, we realize that 
there is insufficient amount of independent and high-quality research on what "fast" means for 
mobile users. Usually, it's in the form of big tech companies boasting about their conversion 
numbers or paid reports based on surveys. 
Regardless, what's even more important is that the feeling of speed is a moving target. With 
access to higher-end and better-performing devices, while using plenty of different apps on 
a single smartphone, users' expectations are growing higher. It's also worth remembering that 
these expectations are vastly dependent on the demographic—e.g., younger generations will 
typically expect apps to load and operate faster than their parents and grandparents, who are 
used to things taking more time. The expectations and incentives to improve are also different 
for internal enterprise app users compared to individual customers.
What will typically give you the most reliable indication of whether your app is fast enough is 
knowing your users and your competition. The analytics you gather on your users' behavior 
will give you personalized insights into places where they tend to drop. Analyzing whether 
your competitors' apps are objectively faster or slower, on the other hand, will hint at whether 
performance is potentially the reason you're losing users.
In the first two parts of this book, we'll focus on how you can improve the runtime performance 
through JavaScript, React, and native optimizations. We will guide you through all the necessary 
tools and techniques you can utilize to better understand what influences FPS so you can fix it 
anywhere.

In the third part, we'll focus on the bundling processes of your app, which happen both on the 
JavaScript and native sides and are highly correlated with the boot-time performance. Similar 
to the first two parts of the guide, you'll be presented with everything you need to have a better 
overview of what's causing your app to load slowly and how to make it fast.

PA RT
JAVASCRIPT
Guides and techniques to improve 
FPS by optimizing JavaScript and 
React side of React Native

A simplified threading model of React Native in New Architecture presenting context in which JS is executed
React Native initialization code handles a handful of key mechanisms:
 ■Initialization of React Native internals—cross-platform C++ React renderer, 
JavaScript Interface (JSI), Hermes JS engine, layout engine (Yoga)—on the main 
thread.
 ■Initialization of the JavaScript thread that runs JS and communicates back and forth 
with the main thread through the JSI.
Introduction
In the first part of this guide, we'll focus on the JavaScript part of the React Native ecosystem. 
As you probably already realize, React Native leverages the native platform primitives and 
glues together various technologies: Kotlin or Java for Android, Swift or Objective-C for iOS, 
C++ for React Native's core runtime, and JavaScript executed by the Hermes engine. It's a lot 
to take in, so let's break it down a bit and focus on one platform for simplicity: Android.
When the user opens a native Android app, it typically goes through a similar initialization 
path: the app opens on the main thread, it loads the native code—usually written in Kotlin, 
Java, or C++—into memory, and then executes this code, which usually results in some kind of 
UI being shown to the user. It's not so different for an Android app created with React Native. 
After all, it's a native app but with a twist: part of the native code being executed comes from 
React Native.

■Initialization of the Native modules thread that runs lazily loaded Turbo Modules.
Notice how rendering logic (Kotlin, C++) and the so-called "business logic" are, by design, run 
on separate threads—the main thread vs. the JS thread. Since this part is about JavaScript and 
React, we'll focus on the JavaScript side that enables React to run on a dedicated JS thread and 
how this model affects the performance of your React Native apps. 
According to an internal survey of 100 React Native developers at Callstack, 80% of the 
performance challenges they face in mobile, TV, and desktop React Native apps originate from 
the JavaScript side. This sample may not be representative of the real world due to its small 
size. However, seeing what the React Native Community is struggling with, it's pretty safe to 
assume that most of the performance issues are related to JavaScript. As such, it's wise to focus 
on these issues before moving to more advanced native optimizations described in Part 2: 
Native.
React re-rendering model
React takes care of rendering and updating the application UI based on its state for you, regardless 
of the environment (or platform): web, iOS, Android, or anything else. The react library 
itself is tiny and consists mostly of public API definitions, some cross-platform functionality, 
and a reconciliation algorithm, which is responsible for efficiently updating the UI to reflect 
component state changes, effectively describing "what to do".
What makes this model powerful, though, is splitting the "what" from the "how" and deferring 
the latter to specialized renderers that manage how these updates are applied to different 
environments: react-dom for the web, react-native for iOS and Android, or react-native-
windows for Windows, to name a few. This model allows you to define components of your 
app universally for all supported platforms simultaneously, and compose the final interface 
out of these smaller building blocks. With such an approach, you don't control the application 
rendering lifecycle; React and its renderer do the job for you.
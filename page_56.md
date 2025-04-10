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
Best Practice: High-Performance Animations Without Dropping Frames
The Ultimate Guide to React Native Optimization
59
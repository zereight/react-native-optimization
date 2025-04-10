BEST PRACTICE
REACT COMPILER
As you already know, many performance issues in React Native apps come from React 
components being re-rendered more often than necessary. To tackle this over-rendering, 
developers can employ various memorization techniques or break out of the React rendering 
model for certain use cases, such as global state management. The problem with hand-written 
memoization is that it makes code harder to read and reason about. There has to be a better 
way, some smart program that would memoize our functions and components automatically. 
And there is one—it's React Compiler.
React Compiler is a new tool from the React core team, designed to automatically optimize 
React applications at build time. It analyzes component structures and applies memoization 
techniques to reduce unnecessary re-renders. Unlike manual optimizations using React.memo, 
useMemo, or useCallback, the compiler automates this process, making it easier to achieve 
optimal performance without additional effort from developers. 
At the time of writing this Guide (January 2025), React Compiler is still in beta. 
Companies like Meta are already using it in production, but whether you can use 
it depends on how well your code follows the Rules of React. If you're eager to try 
it out, you can install the beta version (tagged as 
beta on npm) or experiment with 
daily builds (tagged as experimental).
Preparing your codebase for the React Compiler
Before adding React Compiler to your codebase, it's good to prepare it for the new tool by 
installing an unobtrusive ESLint plugin. The React Compiler ESLint plugin is a helpful tool 
that detects potential issues in real time, flags violations of the Rules of React, and warns about 
optimization blockers.
Even if you're not using the compiler yet, enabling this plugin improves code quality and ensures 
a smoother transition when you decide to adopt it.
Best Practice: React Compiler
The Ultimate Guide to React Native Optimization
52
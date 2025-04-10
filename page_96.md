GUIDE
UNDERSTAND THE 
THREADING MODEL OF TURBO 
MODULES AND FABRIC
Understanding how React Native executes your code is crucial for building efficient native 
modules. In this chapter, we will walk you through the lifecycle of a native module, from 
the initialization phase to calls and de-initialization. By the end of this chapter, you should 
understand which thread is used under what circumstances.
Threading model
Let's start by exploring what threads are available to us in a mobile React Native app:
 ■Main thread / UI thread—whether it's a React Native or plain native app, this thread 
handles UI operations and maintains a responsive user experience.
 ■JavaScript thread—as the name suggests, it's used (although not exclusively) to execute 
JavaScript code).
 ■Native modules thread—a shared pool allocated by React Native specifically for native 
modules.
In addition, you, React Native's renderer, and the third-party modules can create additional 
threads to handle tasks in the background. You can learn more about this topic in the Make 
Your Native Modules Faster chapter.
Turbo Modules
To better understand the threading model of Turbo Modules, let's examine its lifecycle by 
checking how and on which thread a given method is called. 
Guide: Understand the Threading Model of Turbo Modules and Fabric
The Ultimate Guide to React Native Optimization
105
BEST PRACTICE
USE VIEW FLATTENING
React component API is designed to be declarative and reusable through composition. We 
combine multiple components to create complex layouts, much like LEGO blocks. It works 
great on the web, where creating <div/> is quite cheap. React Native operates on native layout 
primitives, which are typically more expensive to process than web equivalents. To mitigate 
this issue, React Native introduced an optimization called "view flattening" to its core renderer. 
It simplifies the view hierarchy where possible to avoid extra memory pressure and CPU 
processing.
This kind of optimization was first introduced for the Android React Native 
renderer.  However,  as  the  project  progressed  with  the  New  Architecture, 
essentially rewriting core parts to C++, it made sense to move this platform-
specific optimization to the shared renderer. Thanks to that, view flattening is 
now possible on iOS too.
Without going into the details of how this works, the renderer identifies "layout-only" nodes. 
They only affect the layout of a 
View and don't render anything on the screen. Those elements 
can be flattened, leading to a depth reduction of the host elements tree. For an in-depth 
explanation of how the view flattening works, check out the official documentation.
Beware of issues with View Flattening
This optimization technique works great in most cases, but sometimes, we want our view 
to stay in the hierarchy. A common case for such a requirement is when you are working on 
a native UI component that accepts children.
Best Practice: Use View Flattening
The Ultimate Guide to React Native Optimization
113
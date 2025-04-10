A picture of React Profiler showing a flame graph representation of React components rendered and re-rendered throughout the 
profiling session
You can press the green View component on the flame graph to "zoom in" to its child 
components. It's an essential UX paradigm implemented by the Profiler, and hopefully, 
you'll learn to enjoy it soon. In this case, you'll notice that both Buttons are rendered in every 
commit, which seems redundant because no props are changing. But are they?
To get an alternative view of the situation, we can access the Ranked view, showing us the 
components from slowest (yellow) to fastest (green). It's pretty similar to a "Bottom-up" view 
you may know from other profiling tools, such as Chrome DevTools or native profilers.
A picture of React Profiler showing a Ranked representation of React components rendered and re-rendered throughout the profiling 
session 
In each view, we can see that a Button component is rendered twice, which reflects our two 
buttons getting re-rendered with every state update.
Looking at our code, this is expected—unless we're using React Compiler, which would 
automatically memoize this code to produce fewer re-renders. So, let's memoize it by hand.
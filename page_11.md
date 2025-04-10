In other words, deciding when to repaint things on screen is purely React's responsibility, while 
deciding about the "how" is the renderer's responsibility. React looks out for the changes you 
have done to your components, compares them, and, by design, only performs the required 
and the smallest number of actual updates.
React component re-renders in the following cases:
 ■Parent component re-renders
 ■State (including hooks) changes
 ■Props change
 ■Context changes
 ■Force update (escape hatch)
Before we move on to the practices that may or may not be relevant to your app's specific use 
case, let's start with something that you will find useful in every app you're going to build: 
profiling and measuring.
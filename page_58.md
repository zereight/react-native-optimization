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
Best Practice: High-Performance Animations Without Dropping Frames
The Ultimate Guide to React Native Optimization
61
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
Best Practice: High-Performance Animations Without Dropping Frames
The Ultimate Guide to React Native Optimization
62
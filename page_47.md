startTransition(() => {
      setSlowCount((prevSlowCount) => prevSlowCount + 1);
    });
  };
  return (
    <SafeAreaView style={styles.container}>
      <CounterNumber count={count} />
      <SlowComponent count={slowCount} />
      {isPending ? <LoadingSpinner /> : null}
      <Button onPress={handleIncrement} title="Increment" />
    </SafeAreaView>
  );
}
export default TransitionScreen;
By separating critical and non-critical updates, the UI remains responsive and delivers a smooth 
user experience. The useTransition hook handles non-urgent updates like the slowCount 
state, ensuring critical updates like the count state for CounterNumber are processed instantly 
while the deferred updates are performed in the background.
How to choose between useDeferredValue and  useTransition
useDeferredValue  is best used for deferring the rendering of a single value. This ensures 
that components relying on that value don't block critical updates. For example, when 
rendering a computationally heavy component based on frequently changing input, 
useDeferredValue ensures the rest of the UI remains responsive.
On the other hand, useTransition defers entire updates or rendering tasks by marking them 
as low-priority. It's most effective when multiple states or components need to be updated 
in the background, such as when transitioning between views or updating a broader section 
of the UI. In React Native apps, view transitions are usually handled by some kind of native 
navigation, such as React Navigation with a native stack navigator. However, some navigators 
are JS-based and could be subject to this optimization technique.
To decide between the two, consider the scope of the updates. If you're managing a single prop 
or value, 
useDeferredValue is likely the better choice. If you're managing transitions that 
involve multiple updates or broader UI changes, useTransition is more appropriate.
Automatic batching
React 18 introduced automatic batching, a process in which React groups multiple state 
updates into a single re-render for better performance. Previously, React only batched updates 
inside React event handlers. Updates inside Promises, setTimeout, native event handlers, or 
any other place were not batched by default by React.
Best Practice: Concurrent React
The Ultimate Guide to React Native Optimization
49
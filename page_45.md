Handling slow components
In many applications, certain components are expensive to render and difficult to optimize—for 
example, when performing complex computations or integrating third-party libraries. Instead 
of allowing these heavy components to block your UI, you can use the useDeferredValue 
hook to prevent them from interfering with higher-priority tasks.
The useDeferredValue hook works like a buffer for your app's updates. Imagine writing a note 
while your friend is reading over your shoulder. If they interrupt you to comment on what you're 
writing, you might need to pause your note-taking momentarily. Similarly, useDeferredValue 
allows React to "pause" rendering less critical updates to ensure user interactions like typing or 
clicking remain smooth.
In practice, it ensures that components relying on frequently changing values don't slow down 
your app. Instead, React prioritizes rendering parts of the UI that matter most at a given time, 
like updating the display as the user types, while deferring heavier computations or updates in 
the background.
import { ActivityIndicator, Button } from "react-native";
import { useDeferredValue, useState } from "react";
import CounterNumber from "@/components/CounterNumber";
import SlowComponent from "@/components/SlowComponent";
function DeferredScreen() {
  const [count, setCount] = useState(0);
  const deferredCount = useDeferredValue(count);
  return (
    <>
      <CounterNumber count={count} />
      <SlowComponent count={deferredCount} />
      {count !== deferredCount ? <ActivityIndicator /> : null}
      <Button onPress={() => setCount(count + 1)} title="Increment" 
/>
    </>
  );
}
export default DeferredScreen;
In the above example, the useDeferredValue hook ensures that updates to SlowComponent 
are deferred while keeping the CounterNumber responsive. The ActivityIndicator appears 
whenever the deferred value lags behind the immediate value, providing clear visual feedback 
to the user. This pattern is particularly useful when rendering expensive components or 
performing time-consuming computations.
Best Practice: Concurrent React
The Ultimate Guide to React Native Optimization
47
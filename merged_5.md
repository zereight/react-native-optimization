if (filter === 'completed') return todo.completed;
    return true;
  });
  return (
    <View>
      {['all', 'active', 'completed'].map((filterType, index) => (
        <FilterMenuItem 
          key={index}
          title={filterType}
          currentFilter={filter}
          onChange={setFilter} 
        />
      ))}
      {/* Todo list section - re-renders entirely when filter 
changes */}
      {filteredTodos.map((todo, index) => (
        <TodoItem key={index} item={todo} onChange={setTodos} />
      ))}
    </View>
  );
};
export default App;
Source with runnable example available at https://snack.expo.dev/@callstack-snack/3762d2
As a result of either filter  or  todos state change, all components: App, FilterMenu, and 
TodoItem will re-render. Updating a TodoItem will cause a re-render of the FilterMenu even 
though it's not dependent on todos. This is not ideal, but if we examine the components in 
the React Native DevTools, we'll see that they're either altered by a hook change or a parent 
component re-render.
A flame graph presenting re-render of the whole UI tree when toggling TodoItem status

To avoid unnecessary re-renders, we can either hand-memoize them using React.memo, 
useMemo and useCallback hooks, or turn on React Compiler to do it for us automatically. 
Or there's a third way—we can break out from the React model and leverage atomic or signal-
based state management libraries.
Atomic state management
Let's focus on the atomic state. It's a bottom-up approach where the state is broken down into 
small, independent units called atoms. Instead of maintaining a large centralized store, each atom 
represents a minimal piece of state that can be updated and subscribed to individually outside 
the React rendering model. This allows for more granular control and better performance 
through targeted re-renders.
There are many libraries that offer atomic, bottom-up state management, such as Zustand, 
Recoil, or Jotai. In the following example, we'll use Jotai.
Jotai means "state" in Japanese, while Zustand means "state" in German.
With Jotai, we define atoms that represent a piece of state. All you need is to specify an initial 
value, which can be primitive or a more complex data structure: 
const filterAtom = atom("all");
const todosAtom = atom(initialState);
We can then use the useAtom hook to get access to the filter atom getter and setter:
const FilterMenuItem = ({ title, filterType }) => (
  const [filter, setFilter] = useAtom(filterAtom);
  
  return (
    <TouchableOpacity onPress={() => setFilter(filterType)}>
      <Text>{title}</Text>
    </TouchableOpacity>
  );
)
Or we can use useSetAtom to only access the setter for todos:
export const TodoItem = ({ item }) => {
  const setTodos = useSetAtom(todosAtom);
  return (
     <TouchableOpacity 
      onPress={() => {

A flame graph presenting re-render of the only affected TodoList component when toggling item status 
Once again, getting out of the React programming model can benefit our app's overall 
performance. For years, atomic state libraries helped us reduce the overall number of re-renders 
while leveraging React APIs such as hooks and useSyncExternalStore under the hood. Now 
that we have access to the React Compiler, it may be unwise to migrate your whole app to 
a new library for the sake of performance when the compiler can do the heavy work for us. You 
can read more about it in the React Compiler chapter.

BEST PRACTICE
CONCURRENT REACT
The perception of how quickly (and smoothly) apps load and respond to user interaction is 
sometimes more important than raw performance—especially when fetching the resources 
from a remote server, which tends to be the slowest part of many apps. While you may not be 
able to make your app run faster physically, you may be able to improve how fast it feels to your 
users. Since it's about relative perception, we call this perceived performance.
A good rule of thumb for improving perceived performance is that it's usually better to provide 
a quick response and regular status updates than make the user wait until an operation is fully 
completed. This is a core idea of Concurrent React.
As a foundational update to React's rendering model, Concurrent React was introduced in 
React 18 and brought to React Native in version 0.69. Instead of completing updates in a single 
uninterrupted process, Concurrent React allows updates to be paused, reprioritized, or even 
abandoned when necessary. This ensures critical interactions like user input are handled as 
a top priority without delay, improving overall user experience.
While concurrency is a behind-the-scenes mechanism, it unlocks several powerful features, 
including interruptible rendering, time-slicing, and automatic batching. These capabilities 
enable React Native to prepare and manage UI updates more efficiently, especially in resource-
intensive scenarios like rendering large datasets or managing complex interactions.
To use Concurrent React features like 
useTransition, useDeferredValue, 
automatic batching, and Suspense for data fetching in React Native, you must 
migrate to the New Architecture, which is the default for new projects starting 
from React Native 0.76.

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

Remember to wrap computation-heavy components you're passing deferred 
values to in React.memo() to prevent unnecessary re-renders caused by parent 
component re-renders.
Presenting stale value while waiting for an update
When dealing with asynchronous work such as data fetching, a pattern that occurs in every app, 
it's very common to show a loading state while waiting for an operation to finish. However, 
this can lead to a jarring user experience where content disappears and is replaced by loading 
indicators frequently.
The useDeferredValue hook offers a more sophisticated approach by allowing you to show 
stale value while the update is not done yet.
import { useState, useDeferredValue, Suspense } from 'react';
import { View, TextInput } from 'react-native';
import SearchResults from './SearchResults';
function SearchScreen() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  return (
    <View>
      <TextInput
        value={query}
        onChangeText={setQuery}
        placeholder="Search items..."
      />
      <Suspense fallback={<LoadingSpinner />}>
        <SearchResults query={deferredQuery} />
      </Suspense>
    </View>
  );
}
During the initial render, the query and deferredQuery values are equal. When the user starts 
typing inside TextInput, the query property updates immediately, but the deferredQuery 
keeps a stale value until the update (search operation) is done.
You can improve visual feedback to better present this experience by introducing a property 
that will keep the value if the search results in this example are stale:
 import { useState, useDeferredValue, Suspense } from 'react';
  import { View, TextInput } from 'react-native';
  import SearchResults from './SearchResults';
  function SearchScreen() {
    const [query, setQuery] = useState('');
    const deferredQuery = useDeferredValue(query);
+   const isStale = query !== deferredQuery;
    return (
      <View>
        <TextInput
          value={query}
          onChangeText={setQuery}
          placeholder="Search items..."
        />
        <Suspense fallback={<LoadingSpinner />}>
-         <SearchResults query={deferredQuery} />
+         <View style={[
+           isStale && { opacity: 0.8 }
+         ]}>
+           <SearchResults query={deferredQuery} />
+         </View>
        </Suspense>
      </View>
    );
  } 
When we wait for an update, the search results will have decreased opacity. Once the update 
finishes, fresh results will be presented, and the defferedQuery will be the same as a query.
Using transitions for non-critical updates
Transitions allow developers to handle non-critical UI updates, such as secondary state changes, 
by marking them as low-priority. This ensures that urgent interactions, like user input, are 
processed first. Unlike useDeferredValue, which defers the rendering of a specific value, 
useTransition defers the entire update process, ensuring smoother management of state 
changes.
import { Button, SafeAreaView, StyleSheet } from "react-native";
import { useState, useTransition } from "react";
import CounterNumber from "@/components/CounterNumber";
import SlowComponent from "@/components/SlowComponent";
function TransitionScreen() {
  const [count, setCount] = useState(0);
  const [slowCount, setSlowCount] = useState(0);
  const [isPending, startTransition] = useTransition();
  const handleIncrement = () => {
    setCount((prevCount) => prevCount + 1);

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

// Before: only React events were batched.
setTimeout(() => {
  setProductQuantity(quantity => quantity + 1);
  setIsCartOpen(isOpen => !isOpen);
  // React will render twice, once for each state update (no 
batching)
}, 1000);
// After: updates inside of timeouts, promises etc.
setTimeout(() => {
  setProductQuantity(quantity => quantity + 1);
  setIsCartOpen(isOpen => !isOpen);
  // React will only re-render once at the end (thanks to 
batching!)
}, 1000);
Automatic batching also prevents your component from being in a "half-finished" state, where 
only one state was updated, which may cause unexpected visual glitches. 
If you want to learn more about this topic, we highly recommend reading this 
page inside the official React documentation. 
Concurrent React features, including ⁠useDeferredValue, ⁠useTransition, and automatic 
batching, enable better-perceived performance by intelligently managing rendering tasks 
and state updates without blocking the UI. These powerful capabilities can be incrementally 
adopted in your apps, allowing you to enhance performance and responsiveness where needed 
most, especially in resource-intensive scenarios.

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
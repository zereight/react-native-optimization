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
Best Practice: Concurrent React
The Ultimate Guide to React Native Optimization
48
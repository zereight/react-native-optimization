BEST PRACTICE
ATOMIC STATE MANAGEMENT
A React app will re-create itself from scratch each time, based on the inputs it gets in the form 
of state, props, or context. Any React component will re-render itself based on these inputs 
and when its parent component updates. This is the promise of React that we enjoy so much 
as developers because it gives us predictability and certainty that our changes are always in sync 
with the whole app. 
However, the same mechanism often causes performance issues due to unnecessary re-renders 
that are propagated further and further into a component tree if not carefully optimized. At 
least as long as we don't use React Compiler.
The higher a re-render occurs in the component tree, the more likely it is to propagate changes 
to leaf components unnecessarily. This is the case with the global app state, which we'll find 
in almost any React app. If not optimized, a change in the global store will cause re-renders in 
components that are not even affected by this change. 
We often observe it in codebases that rely heavily on React's Context or external libraries such 
as Redux. 
Let's examine a small code snippet illustrating an 
App component that holds state for filter 
and todos, which are used by FilterMenuItem and TodoItemList components that are not 
memoized.
import React, { useState } from 'react';
import { View, Text, TouchableOpacity } from 'react-native';
const App = () => {
  const [filter, setFilter] = useState('all');
  const [todos, setTodos] = useState(initialState);
  
  const filteredTodos = todos.filter(todo => {
    if (filter === 'active') return !todo.completed;
Best Practice: Atomic State Management
The Ultimate Guide to React Native Optimization
42
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
Best Practice: Atomic State Management
The Ultimate Guide to React Native Optimization
43
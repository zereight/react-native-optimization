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
Best Practice: Atomic State Management
The Ultimate Guide to React Native Optimization
44
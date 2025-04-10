import { c as _c } from 'react/compiler-runtime';
export default function MyApp() {
  const $ = _c(2);
  const [value, setValue] = useState('');
  let t0;
  if ($[0] !== value) {
    t0 = <TextInput onChangeText={() => setValue(value)}>Hello 
World</TextInput>;
    $[0] = value;
    $[1] = t0;
  } else {
    t0 = $[1];
  }
  return t0;
}
After running compiler: more code, less re-renders
Notice how React Compiler transformed our code. It imports a c function from the react/
compiler-runtime and uses it to determine what to render based on state. The c(n) function is 
a polyfill of useMemoCache(n) from React 19, allowing the same behavior in earlier versions of 
React. It creates an array that persists across renders, similar to useRef. If we were to implement 
it using useRef, it would look like this:
function useMemoCache(n) {
  const ref = useRef(Array(n).fill(undefined));
  return ref.current;
}
useMemoCache creates an array with n slots that persist across renders
In the example above, const  $  =  _c(2) works like useMemoCache(2), returning an array 
with two slots. $[0] holds the previous value, while $[1] stores the last rendered TextInput. 
When 
value changes, a new TextInput is stored in $[1]. If value remains the same, React 
reuses the cached component instead of re-rendering it.
React Compiler uses shallow comparison, just like React.memo and useMemo, so 
be careful when using objects or arrays as props. If their reference changes, they 
will be treated as new values.
React Compiler Playground
If you want to understand how the React Compiler transforms your code, the React Compiler 
Playground is your friend. It allows you to inspect how the compiler optimizes components, test 
different structures, and debug compiler output. This interactive environment helps you see 
what changes the compiler makes and identify any unexpected behaviors in your components.
Best Practice: React Compiler
The Ultimate Guide to React Native Optimization
55
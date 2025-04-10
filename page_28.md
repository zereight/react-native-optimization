BEST PRACTICE
UNCONTROLLED COMPONENTS
The React programming model revolves around the idea of re-rendering the whole app—
represented by a tree of components—every time there's a state change. However, while this 
model works for most use cases in UI programming, it's only an abstraction. And, as with 
any other abstraction, it simplifies reality so that it's easier to reason about but at the cost 
of correctness. A good abstraction will contain escape hatches—it's like permission from its 
authors to venture off of it to achieve our goals as programmers. 
React is no different and offers a variety of escape hatches, such as refs, which allow bypassing 
Reacts re-rendering logic and open the door for components that are not driven by state 
updates. We call these "uncontrolled components", and it's a powerful pattern we'll explore in 
this chapter, especially in the context of the legacy asynchronous architecture of React Native.
Controlled TextInput in legacy architecture
Almost every React Native application you'll write will contain some form of input, such as 
text or voice. Let's focus on text, which is by far the most common and, in React Native apps, is 
represented by the TextInput component. This component can be either controlled—through 
React props—or uncontrolled, leveraging refs and callback props to communicate back to the 
React model.
Let's see an example of such a controlled text input first:
const DummyTextInput = () => {
  const [value, setValue] = useState("Text");
  const onChangeText = (text) => {
    setValue(text);
  };
  return (
    <TextInput 
Best Practice: Uncontrolled Components
The Ultimate Guide to React Native Optimization
30
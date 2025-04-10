A diagram showing what happens while typing TEST too fast
When the updates via onChangeText arrive before React Native synchronizes each of them 
back, the interface starts flickering. The first update (operation 1 and operation 2) performs 
without issues as the user starts typing T.
Next, operation 3 arrives, followed by operation 4. The user typed E & S while React Native 
was busy doing something else, delaying the synchronization of the letter E (operation 5). As 
a result, the native input will temporarily change its value back from TES to TE.
Now, the user was typing fast enough to actually enter another character when the value of 
the text input was set to TE for a second. As a result, another update arrived (operation 6) with 
the value of TET. This wasn't intentional—the user wasn't expecting the value of its input to 
change from TES to TE.
Finally, operation 7 synchronized the input back to the correct input received from the user 
a few characters before (operation 4 informed us about TES). Unfortunately, it was quickly 
overwritten by another update (operation 8), which synchronized the value to TET—the final 
value of the input.
The root cause of this situation lies in the order of operations. Things would have run smoothly 
if operation 5 had been executed before operation 4. Also, if the user hadn't typed 
T when the 
value was TE instead of TES, the interface would have flickered, but the input value would have 
remained correct.
Uncontrolled TextInput
One solution for the synchronization problem is to remove the value prop from TextInput 
entirely and make it an uncontrolled component. 
const DummyTextInput = () => {
  const [value, setValue] = useState("Text");
  const onChangeText = (text) => {
Best Practice: Uncontrolled Components
The Ultimate Guide to React Native Optimization
32
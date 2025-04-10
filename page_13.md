<Text>Welcome!</Text>
  <Text>{count}</Text>
  <Text>{second}</Text>
  <Button onPress={() => setCount(count + 1)} title="Press one" 
/>
  <Button onPress={() => setSecond(second + 1)} title="Press 
two" />
    </View>
  );
};
const Button = ({onPress, title}) => {
  return (
    <Pressable style={styles.button} onPress={onPress}>
      <Text>{title}</Text>
    </Pressable>
  );
};
When rendered, the <App /> component will display a welcome message with two counters 
and buttons controlled by two separate state handlers. We're now ready to profile this React 
code with React Native DevTools. 
You can access it by either pressing j in the Metro dev server or from the app using the React 
Native Dev Menu—accessed through a shake gesture on a device, Cmd+M on Android or Cmd+D 
on iOS—and pressing "Open DevTools". It will present you with a welcome screen, pointing 
to learning resources on the basics of debugging, DevTools features, and native debugging. We 
highly recommend getting familiar with these resources, some overlapping with the materials 
presented in this guide.
 React Native DevTools welcome screen 
Guide: How to Profile JS and React Code
The Ultimate Guide to React Native Optimization
15
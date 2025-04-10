In other words, deciding when to repaint things on screen is purely React's responsibility, while 
deciding about the "how" is the renderer's responsibility. React looks out for the changes you 
have done to your components, compares them, and, by design, only performs the required 
and the smallest number of actual updates.
React component re-renders in the following cases:
 ■Parent component re-renders
 ■State (including hooks) changes
 ■Props change
 ■Context changes
 ■Force update (escape hatch)
Before we move on to the practices that may or may not be relevant to your app's specific use 
case, let's start with something that you will find useful in every app you're going to build: 
profiling and measuring.

GUIDE
HOW TO PROFILE JS 
AND REACT CODE
When optimizing performance, we want to know exactly which code path is the source of 
a slowdown. With some experience and knowledge of the codebase and common performance 
pitfalls, developers can often identify the issues intuitively and expect the app to run faster. In 
reality, though, apps are usually too complex for anyone to know the whole system by heart. 
Typical blind optimizations we make have little impact on the overall performance. To be 
precise with our actions, we must make decisions guided by data. 
The data comes from measuring performance using specialized tools. This process is often 
referred to as "profiling", as it involves analyzing a program to create a "profile" of its execution. 
This profile typically includes information about which parts of the program consume the 
most resources, such as CPU time or memory, helping identify performance bottlenecks.
Profiling with React Native DevTools
In the context of a React Native app, it's helpful to be able to profile React itself to inspect 
unnecessary re-renders. The best tool to profile React running in a mobile app is React Native 
DevTools, and that's what we'll focus on in this guide. 
React Profiler is integrated as the default plugin in React Native DevTools and can produce 
a flame graph of the React rendering pipeline as a result of profiling. We can leverage this data 
to analyze the app's re-rendering issues. 
Here is the code we're about to profile:
export const App = () => {
  const [count, setCount] = React.useState(0);
  const [second, setSecond] = React.useState(0);
  return (
    <View style={styles.container}>

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

Now, open the React Native DevTools. Go to the Profiler tab on top. Press the "gear" button 
to configure it to "Highlight updates when components render"
General profiler settings accessed from "View Settings" icon in the Profiler tab of React Native DevTools
and "Record why each component rendered while profiling".
Profiler settings accessed from "View Settings" icon in the Profiler tab of React Native DevTool
Finally, press the "Start profiling" or "Reload and start profiling" button on the top left of the 
screen to start profiling. It doesn't matter in our use case since we're inspecting reactions to 
user input. However, when profiling an app's startup, "Reload and start profiling" is a lifesaver.
"Start profiling" button.
In our sample app, click both buttons a few times and observe real-time feedback from the 
DevTools highlighting components that are being re-rendered. The screenshots reflect a profile 
produced by first pressing "Press one" twice and then "Press two" twice, producing four 
subsequent "commits" by the React renderer.
You can learn more about React's render and commit process in the official docs.

A picture of React Profiler showing a flame graph representation of React components rendered and re-rendered throughout the 
profiling session
You can press the green View component on the flame graph to "zoom in" to its child 
components. It's an essential UX paradigm implemented by the Profiler, and hopefully, 
you'll learn to enjoy it soon. In this case, you'll notice that both Buttons are rendered in every 
commit, which seems redundant because no props are changing. But are they?
To get an alternative view of the situation, we can access the Ranked view, showing us the 
components from slowest (yellow) to fastest (green). It's pretty similar to a "Bottom-up" view 
you may know from other profiling tools, such as Chrome DevTools or native profilers.
A picture of React Profiler showing a Ranked representation of React components rendered and re-rendered throughout the profiling 
session 
In each view, we can see that a Button component is rendered twice, which reflects our two 
buttons getting re-rendered with every state update.
Looking at our code, this is expected—unless we're using React Compiler, which would 
automatically memoize this code to produce fewer re-renders. So, let's memoize it by hand.

// wrap inline functions with `useCallback`
const onPressHandler = useCallback(() => setCount(count + 1), 
[count]);
const secondHandler = useCallback(() => setSecond(second + 1), 
[second]);
// ...
<Button onPress={onPressHandler} title="Press one" />
<Button onPress={secondHandler} title="Press two" />
// wrap Button component with `memo`
const Button = memo(({onPress, title}) => {
  // ...
});
After wrapping Button with React.memo and its onPress handlers with React.useCallback, 
we get a different picture from the profiler, showing the same number of 4 React commits. 
However, now, only the button that's actually pressed is re-rendered, while the other one 
gets memoized (with auto-generated _c2 name), which is indicated by the green color of this 
component and its children. 
A flame graph of the same flow but no with one of the buttons not being updated due to memoization 
Now, you're well-equipped to start your adventure with profiling any React application—
whether it's a web app or mobile app. We strongly advise you to take a break from reading now 
and try it in the apps you develop on a daily basis. You'll notice the flame graph output will be 
much noisier due to the bigger complexity of your React app. 
Remember to focus on the components marked with yellow as the ones where 
React spends the most time. And leverage the "Why did this render?" information. 
Profiling JavaScript Code
React Native DevTools provide great utilities for profiling not only React but also JavaScript 
code. After all, it's based on Chrome DevTools, reusing its backend. In this section, we will 

focus on profiling the CPU, examining what's on the call stack, and determining how you can 
detect long-running operations. If you want to inspect memory-related issues, you can check 
the How to Hunt JS Memory Leaks chapter.
After launching React Native DevTools, you can go to the "JavaScript Profiler" Tab and start 
recording the JavaScript CPU Profile.
If you don't see the "JavaScript Profiler" tab, navigate to settings by clicking on 
the gear icon at the top right, and from the experiments section, enable JavaScript 
Profiler.
At the top left, you can select the preferred view. You can choose from Chart, Heavy (Bottom-
Up), or Tree. Depending on what you want to achieve, experiment with each view.
Let's experiment with this long-running function:
  const longRunningFunction = () => {
    let i = 0;
    while (i < 1000000000) {
      i++;
    }
  };
Start recording the profile and stop once it's finished. Once the profile is created, we can see that 
the longRunningFunction took over 12 seconds to run, combined with the whole call stack, 
which led to calling this function. This effectively makes our JS part of the app unresponsive 
for those 12 seconds, but the native UI part should still be responsive due to React Native's 
threading model. 
In mobile apps, we want our functions never to block the JS thread for more than 
16 ms to achieve 60 FPS and, ideally, 8 ms for 120 FPS, which is getting more 
popular. Read more about FPS in the How to Measure JS FPS chapter.
Profiler output showing a flame chart with long-running longRunningFunction taking over 12 seconds to run
If the Chart (or flame graph, as React DevTools calls it) information is hard to read, you can try 
other views, such as Heavy (Bottom Up), where you'll be able to sort by the longest or shortest 
running function calls.

Heavy (Bottom Up) view of the same profile information, longRunningFunction is at the top
Profiler makes it easier to spot functions that run longer than expected, tracking them down 
with surgical precision and realizing where a function gets called. This superpower is worth 
leveraging in your day-to-day work as a software engineer.

GUIDE
HOW TO MEASURE JS FPS
Mobile users expect smooth, well-designed interfaces that respond instantly and provide clear 
visual feedback. As a result, applications often include numerous animations that must run 
alongside other processes, such as data fetching and state management. If your application fails 
to provide a responsive interface, lags user input, or feels "janky" at times, it's a sign the UI is 
"dropping frames". Users hate it.
What is FPS
Getting your code to display anything on the screen involves compiling the code to an executable 
format, which, using various techniques, draws pixels on the screen. A single drawing is called 
a "frame". If you want your UI not to be static—which you do—your app will need to draw 
many frames every second. Most mobile devices have a refresh rate of 60 Hz, meaning they can 
display 60 frames per second, or 60 FPS. That's our metric.

Most people perceive 60 FPS as smooth motion. However, as technology advances, users have 
access to screens that can refresh 120 or even 240 times a second. Once a user enjoys a 120 Hz 
screen, chances are they'll consider 60 Hz "choppy," "laggy," or "slow". It means that our target 
of 16,6 ms to render a frame (1/60 s) is moving, and we should aim for 8,3 ms per frame.
If you fail to think about your app's performance and choose the right tools to 
tackle this challenge, you're on the right track to dropping frames sooner or later.
React Perf Monitor
Thankfully, React Native comes with a handy tool called React Perf Monitor, which can display 
FPS live as an overlay on top of your app. You can open it by selecting it from the React Native 
Dev Menu.
You can open DevMenu by shaking the device or by using dedicated keyboard 
shortcuts:
 ■iOS Simulator: Ctrl + Cmd ⌘ + Z (or Device> Shake).
 ■Android emulators: (Cmd ⌘ / Ctrl )+ M.
 React Native Dev Menu drawer  Perf Monitor visible in the top-left corner
It opens a pane in the upper left corner of the application. You can hide it with the "Hide Perf 
Monitor" option by accessing the React DevMenu once again.
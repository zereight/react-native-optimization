> flashlight measure
This allows you to monitor memory usage and when the FPS slows down. There are actually 
two FPS monitors—one for the UI (or Main) thread and another for the JS thread. This allows 
you to quickly realize whether a particular interaction slowdown originates from JavaScript or 
native.
Always disable development mode when measuring performance. 
On Android, it's accessible from Dev Menu in Settings > JS Dev Mode.
On iOS, you'll need to run Metro in dev mode.
As we learned in the How to Profile JS and React Code chapter, when the FPS drops during 
animation or some operation, you can use a profiler to narrow down the cause.
Flashlight
FPS in the Dev Menu is easy to see but hard to track and measure. It's quite convenient to get 
a view of, e.g., the average FPS for a particular interaction or screen load. This is where tools 
like Flashlight shine. It works only on Android, though.
To get started, make sure you have Flashlight installed and available in your terminal. Open up 
the Android application you want to measure on a given Android emulator, and then run: 
You will see the diagram with various performance metrics appear in real time as you interact 
with the application. 
Getting an average FPS is only one of the features available in a lighthouse-like 
report produced by the tool. The report also includes the app's overall performance 
score based on the metrics collected during the measurement, such as the average 
FPS number, average CPU, and RAM usage.
All collected measurements are saved in a JSON file that you can store and compare between 
different versions of your code. It's a good solution for automated local performance monitoring 
and to prove that the FPS has improved reliably.

GUIDE
HOW TO HUNT JS 
MEMORY LEAKS
When a program is executed on a target device or virtual machine, it always occupies some 
space in the device's Random Access Memory (RAM), which is allocated specifically for that 
program to execute correctly. When that memory allocation is unexpectedly retained, we say 
it's "leaking"; hence, we deal with memory leaks.
Memory leaks can be hard to trace without the right tools. In most cases, they arise from 
programmer errors. If your app constantly uses more and more memory without releasing 
it, you most likely have a leak somewhere. You might try to play the guessing game of what is 
leaking, but it's way easier to profile your app with the right tools that will help you find it. 
Let's start by defining what memory leaks are.
Anatomy of a memory leak in a JavaScript app
JavaScript engines, such as Hermes—and any other interpreted languages that leverage a virtual 
machine to execute—implement a special program called garbage collector (GC). As the name 
implies, it "collects garbage", where "garbage" refers to memory addresses that are no longer 
needed for the application to run and can be freed for other programs to use. GC periodically 
scans allocated objects and determines if an object is no longer needed. Fun fact: Hermes' 
garbage collector is called Hades.
Although garbage collectors use advanced techniques to reliably and safely determine which 
memory to free, in some cases, your code may sometimes trick the GC into keeping an object 
allocated when it shouldn't be. That's how you end up with memory leaks that are often hard 
to track without dedicated tools.
In short, a memory leak happens when a program fails to release the memory that 
is no longer needed.

Here are a few examples of code that leaks memory for better understanding.
1. Listeners that don't stop listening:
const BadEventComponent = () => { 
  useEffect(() => {
    const subscription = EventEmitter.addListener('myEvent', 
handleEvent);
    // Missing cleanup function
  }, []);
  return <Text>Listening for events...</Text>;
};
2. Timers that don't stop counting:
const BadEventComponent = () => {
  useEffect(() => {
    const subscription = EventEmitter.addListener('myEvent', 
handleEvent);
    // Missing cleanup function
  }, []);
  return <Text>Listening for events...</Text>;
};
3. Closures that retain a reference to large objects:
class BadClosureExample {
  private largeData: string[] = new Array(1000000).fill('some 
data');
  createLeakyFunction(): () => number {
    // Entire largeData array is captured in closure
    return () => this.largeData.length;
  }
}
// Fixed
class GoodClosureExample {
  private largeData: string[] = new Array(1000000).fill('some 
data');
  createEfficientFunction(): () => number {
    // Only capture the length, not the entire array
    const length = this.largeData.length;
    return () => length;
  }
}

Now that you know what typical memory leaks look like, let's see what tools you can use 
to spot them easily.
Hunting memory leaks with React Native DevTools
The new React Native DevTools builds on top of Chrome DevTools, so if you are familiar 
with web debugging, you should feel right at home. However, if you want to learn more about 
Chrome DevTools, you should check out their official documentation.
React Native DevTools offers three ways to profile your app's memory: 
 ■Heap snapshot—shows memory distribution among your page's JavaScript objects 
and related DOM objects.
 ■Allocation instrumentation on the timeline—this profile type shows instrumented 
JavaScript memory allocations over time. Once a profile is recorded, you can select a 
time interval to see objects that were allocated within it and still live by the end of the 
recording. Use this profile type to isolate memory leaks.
 ■Allocation sampling—records memory allocations using the sampling method. This 
profile type  has minimal performance overhead and can be used for long-running 
operations. It provides good approximations of allocations broken down by the 
JavaScript execution stack.
We'll focus on Allocation  instrumentation  on  the  timeline,  which should help us find 
allocations that are not being freed up. Let's run it on this piece of code: 
// Global variable to store closures
let leakyClosures: Funciton[] = [];
// Generate some dummy data
const generateLargeData = () => {
  return new Array(1000).fill(null).map(() => ({
    id: Math.random().toString(),
    data: new Array(500).fill('??').join(''),
    timestamp: new Date().toISOString(),
    nested: {
      moreData: new Array(100).fill({
        value: Math.random(),
        text: 'nested data that consumes memory',
      }),
    },
  }));
};
const createClosure = () => {
  const newDataLeak = generateLargeData();  // Creates large data 
array
  return () => {

newDataLeak.forEach((data) => data.id); // Closure captures 
newDataLeak
  };
};
// Create many closures that each capture their own large data
const createManyLeaks = () => {
  for (let i = 0; i < 10; i++) {
    const leakyClosure = createClosure();
    leakyClosures.push(leakyClosure);  // Store reference to 
prevent GC
    // Trigger all closures to keep data "active"
    leakyClosures.forEach(closure => closure());
  }
};
Code showing the third issue from the previous section (closures retaining reference to large data) 
Once you launch DevTools, look for the "Memory" tab and select "Allocation instrumentation 
on timeline". Then, you can click "Start" at the bottom.
Memory panel in React Native DevTools
You have a leak when you finish profiling, and an object is still allocated in memory! But as 
mentioned before, GC runs periodically, so your objects might not get deallocated instantly. 
You might need to allocate something else to trigger the deallocation of existing objects.

Here is what the profiler looks like after capturing a snapshot:
Output from the Allocation instrumentation tool showing a potential memory leak with blue bars 
There are a few things to keep in mind about the Allocation instrumentation tool: 
 ■Blue bars at the timeline represent allocations.
 ■If the blue bar turns grey, the object is deallocated.
 ■Below, we have a list of list of constructors that were called. Right next to it, there are 
two metrics:
 ■Shallow size—the size of memory that is held by the object.
 ■Retained size—the memory size that will be freed after the object is deleted.
As you can see, over 30 seconds and multiple calls to the createManyLeaks function, GC 
cleared only some objects from the memory (shown on the graph as grey), but the blue parts 
are still alive. This graph screams that multiple leaks are happening.
Let's focus on the first spike and investigate what's causing this. 
Focused view of allocation spike
Now that the graph is focused on the first spike, we can see which constructors are being called. 
Let's expand the Function constructor and see what we have there.

The discrepancy between shallow and retained size for JSFunction
As you can see, there is a call to the JSFunction() constructor whose shallow size is 40 bytes, 
but the retained size is 2 772 088 bytes(!). It's the leak that we were looking for! This closure 
is tricking GC into thinking that the objects are still relevant when they are not. DevTools even 
show us the exact line where this constructor was called. 
After finding the leak, you should fix it and re-test. Let's alter the createManyLeaks function 
and comment-out pushing closures to an array to prevent retaining them.
const createManyLeaks = () => {
  for (let i = 0; i < 5; i++) {
    const leakyClosure = createLeak();
    leakyClosure(); // just call it
    // leakyClosures.push(leakyClosure); 
    // leakyClosures.forEach(closure => closure());
  }
}
As you can see, all the bars are now grey (except the last one, which wasn't yet deallocated), 
which means there are no leaks!
Memory allocation profile with no leaks 
Now you know how to track down and fix memory leaks! In the next chapter, we will focus on 
Controlled Components.

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

A diagram showing what happens while typing TEST
As soon as the user starts inputting a new character into the native input, an update is sent to 
React Native via the onChangeText  prop (operation 1 on the diagram above). React processes 
that information and updates its state accordingly by calling setState. Next, a controlled 
component synchronizes its JavaScript value with the native component value (operation 2 on 
the diagram above).
There are benefits to such an approach. React is a source of truth that dictates the value of your 
inputs. This technique lets you alter the user input as it happens by performing a validation, 
masking it, or completely modifying it.
Unfortunately, while ultimately cleaner and more compliant with the way React works, the 
approach mentioned above has one downside. It is most noticeable when there are limited 
resources available and/or the user is typing at a very high rate. 
      onChangeText={onChangeText} 
      value={value} 
    />
  );
};
The above code sample will cause the native text input to update its value through the React 
model whenever a user inputs text. The speed at which input comes is often quite fast or even 
guided by some autocomplete. Pair that with a low-end Android device or an app doing some 
heavy computing while the user is typing, and your users may end up with lags and dropped 
frames. Even worse, in legacy asynchronous architecture, your React state may get out of sync 
with the native input state, causing unexpected behaviors. 
The problem with controlled TextInput de-synchronization shouldn't exist in 
New Architecture. 
To better understand what is happening here, let's first look at the order of standard operations 
that occur while the user is typing and populating your <TextInput /> with new characters.
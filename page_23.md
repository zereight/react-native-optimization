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
Guide: How to Hunt JS Memory Leaks
The Ultimate Guide to React Native Optimization
25
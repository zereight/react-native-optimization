// Before: only React events were batched.
setTimeout(() => {
  setProductQuantity(quantity => quantity + 1);
  setIsCartOpen(isOpen => !isOpen);
  // React will render twice, once for each state update (no 
batching)
}, 1000);
// After: updates inside of timeouts, promises etc.
setTimeout(() => {
  setProductQuantity(quantity => quantity + 1);
  setIsCartOpen(isOpen => !isOpen);
  // React will only re-render once at the end (thanks to 
batching!)
}, 1000);
Automatic batching also prevents your component from being in a "half-finished" state, where 
only one state was updated, which may cause unexpected visual glitches. 
If you want to learn more about this topic, we highly recommend reading this 
page inside the official React documentation. 
Concurrent React features, including ⁠useDeferredValue, ⁠useTransition, and automatic 
batching, enable better-perceived performance by intelligently managing rendering tasks 
and state updates without blocking the UI. These powerful capabilities can be incrementally 
adopted in your apps, allowing you to enhance performance and responsiveness where needed 
most, especially in resource-intensive scenarios.
Best Practice: Concurrent React
The Ultimate Guide to React Native Optimization
50
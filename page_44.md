BEST PRACTICE
CONCURRENT REACT
The perception of how quickly (and smoothly) apps load and respond to user interaction is 
sometimes more important than raw performance—especially when fetching the resources 
from a remote server, which tends to be the slowest part of many apps. While you may not be 
able to make your app run faster physically, you may be able to improve how fast it feels to your 
users. Since it's about relative perception, we call this perceived performance.
A good rule of thumb for improving perceived performance is that it's usually better to provide 
a quick response and regular status updates than make the user wait until an operation is fully 
completed. This is a core idea of Concurrent React.
As a foundational update to React's rendering model, Concurrent React was introduced in 
React 18 and brought to React Native in version 0.69. Instead of completing updates in a single 
uninterrupted process, Concurrent React allows updates to be paused, reprioritized, or even 
abandoned when necessary. This ensures critical interactions like user input are handled as 
a top priority without delay, improving overall user experience.
While concurrency is a behind-the-scenes mechanism, it unlocks several powerful features, 
including interruptible rendering, time-slicing, and automatic batching. These capabilities 
enable React Native to prepare and manage UI updates more efficiently, especially in resource-
intensive scenarios like rendering large datasets or managing complex interactions.
To use Concurrent React features like 
useTransition, useDeferredValue, 
automatic batching, and Suspense for data fetching in React Native, you must 
migrate to the New Architecture, which is the default for new projects starting 
from React Native 0.76.
Best Practice: Concurrent React
The Ultimate Guide to React Native Optimization
46
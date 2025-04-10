bigger overhead than the stack. At the same time, you need to be cautious of how much you 
allocate on the stack, or else you risk... a stack overflow! But if possible, use stack and resort to 
allocating on the heap when necessary.
Reference cycles
The most common source of memory leaks when using reference counting are reference cycles, 
also known as circular references. Let's consider a simple example:
class A {
  std::shared_ptr<B> b;
};
class B {
  std::shared_ptr<A> a;
};
C++
class A {
 var b: B?
}
class B {
  var a: A?
}
Swift
Guide: Understanding Native Memory Management
The Ultimate Guide to React Native Optimization
100
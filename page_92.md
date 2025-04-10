In this example, class A depends on class B, which in turn depends on class A, causing a reference 
cycle.
It's worth noting that the reference cycles will be more complex in real-world 
scenarios; the cycles can span multiple classes, which makes them harder to find 
and fix.
The solution is to break the cycle by introducing a weak pointer (std::weak_ptr<T>). A weak 
pointer is an optional dependency that does not block the dependent object from being removed 
if it's no longer used. Whenever we want to access the dependency, we need to check if it still 
exists, as it is no longer guaranteed:
class A {
  std::shared_ptr<B> b;
};
class B {
  std::weak_ptr<A> a;
};
C++
class A {
 var b: B?
}
class B {
  weak var a: A?
}
Swift
Alternatively, we could introduce a third class, C, holding the shared resource that classes A and 
B depend on, which would help us avoid the cycle. This refactoring technique is commonly 
used in JavaScript codebases.
Not freed-up resources
In languages using garbage collector mechanisms, we can often introduce memory leaks by not 
freeing up resources. This can be observed when using the listener pattern and forgetting to 
call to remove them. If you come from a JavaScript background, you probably already know it 
all too well. Let's look at an example that showcases this common issue. First, create a simple 
DataManager class responsible for registering and calling listeners:
Guide: Understanding Native Memory Management
The Ultimate Guide to React Native Optimization
101